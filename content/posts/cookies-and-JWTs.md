+++
date = '2025-10-18T15:16:22+01:00'
title = 'Cookies and JWTs - Use Cases and Practical Example in C#'
+++
Welcome to the third and final part of my Identity and Access Management (IAM) trilogy. In the last two parts, we defined what authentication and authorization are. We also discussed two very important protocols for managing authentication and authorization via third-party apps like OAuth 2.0 and OpenID Connect.

This covers most of the key aspects of authentication and authorization. However, there is still one important puzzle piece missing, and that is persisting a session after a user logs in.

To really understand this problem, imagine a user authenticates via the login form and then refreshes the page. Without implementing any mechanism to persist the session after login, the user still can’t authorize herself for anything because the backend requires something that actually proves the user has authenticated themselves at some point. This is where cookies and JSON Web Tokens (JWTs) come in.

## Prerequisites

* Authentication and Authorization

# What you should know about cookies

Session cookies are stateful elements. They contain data that the server sends to the browser for temporary use. The authentication data inside a cookie is stored on both the client and server. The server keeps track of active sessions in a database, while the browser holds the identifier to the active session. When a request is made to the server, the session ID is used to look up information such as user roles or privileges for authentication, in order to check if the session is valid.

## Advantages of cookies

Consider these key benefits:

* Cookies use the same session across subdomains: They take a Domain argument. You specify the domain name for which the cookie is valid. Setting the domain name to yoursite.com allows the same session for the domain and subdomains.
* They reduce the manipulation by client-side JavaScript: You can restrict client-side access by setting the HttpOnly flag when creating the cookie on your backend. This reduces the likelihood of cross-site scripting (XSS) attack on your application, since most XSS attacks involve the use of malicious JS code.
* They require little storage: Cookies use as little as 6 KB to store a simple user ID. Depending on what information you store in your cookie, you’ll transmit a minimal amount of bytes with every request.
* Cookies are managed by the browser: This is automatic, so you don’t have to worry about it.

## The downside of cookies

Some of the disadvantages of cookies include:

* Cross-site request forgery attacks (XSRF or CSRF): CSRF attacks are only possible with cookie-based session handling. The SameSite attribute allows you to decide whether cookies should be sent to third-party apps using the Strict or Lax settings. A strict setting can prevent CSRF attacks, but it can also contribute to a poor browser experience for the user. For example, say your site uses a cookie named tutorials_shown to determine wheather a user has already seen specific tutorials in order to show them new ones every time if they visit. If SameSite is set to Strict and someone follows a link to your site, the cookie will not be sent on that first request, and previously viewed tutorials will be shown. This creates a less personalized user experience.
* Scaling issues: Since sessions are tied to a particular server, you can run into issues when scaling your application. In a load-balanced application, if a logged-in user is redirected to a new server, the existing session data is lost. To prevent this, sessions need to be stored in a shared database or cache. This increases the complexity of each interaction.
* Not good for API authentication: APIs provide one-time resources for authenticated end-users and don’t need to keep track of user sessions. Cookies don’t work perfectly in this case, since they track and verify active sessions. Tokens, meanwhile, provide authentication with a unique identifier on every request to the API endpoints.

Cookies aren’t the only way to store session IDs; other options include URLs and form fields. Cookies are more secure than those two, but how secure are cookies?

* Cookies are only secure in an HTTPS connection. Enforcing the Secure flag ensures that cookies are only sent via an encrypted HTTPS connection. Use of HTTPS prevents disclosure of session ID in man-in-the-middle (MITM) attacks.
* As noted earlier, cookies can be manipulated by client-side scripts (JavaScript or Visual Basic). This can be prevented by using the HttpOnly flag.

While cookies can be made secure by setting the appropriate attributes and following best practices, they can also be made insecure by neglecting these steps.

# JSON Web Tokens

Tokens—or JWTs in this context—are stateless in nature, meaning the server doesn’t need to keep a record of the token. Each token is self-contained, holding the information needed for verification and identification on the server.

## Advantages of tokens

Here are some specific advantages of tokens:

* Flexibility and ease of use: JWTs are easy to use. Their self-contained nature helps you achieve what you need for verification without database lookups. This makes JWTs more suitable to use in an API, since the API server doesn’t need to keep track of user sessions.
* Cross-platform capabilities: Because of their stateless nature, tokens can be seamlessly implemented on mobile platforms and internet of things (IoT) applications, especially in comparison to cookies…
* Multiple storage options: Tokens can be stored in a number of ways in browsers or front-end applications.

If you use a browser’s local storage, tokens can’t be accessed by a subdomain. However, they can be accessed and manipulated by any JavaScript code on the webpage, as well as by browser plugins. This isn’t a recommended method: first, it poses a security risk, plus you must manage the storage.

Session storage is another way to store tokens. The drawback is that the token is destroyed when the browser is closed.

## Disadvantages of JWT tokens

Here are some downsides of tokens to be aware of:

* Revocation: A JWT cannot be revoked. Even if a JWT leaks, it remains valid until it expires, resulting in a serious security hole. As a workaround, you must implement a deny-list technique that requires a more complex setup.
* Need more space: A JWT might need 300+ bytes to store a simple user ID, because they store other data for authentication.
* Stale: The information inside a JWT represents a snapshot in time when the token was originally created. The associated user may now have different access levels or have been removed from the system altogether.

## But what about the security of tokens?

* JWTs are cryptographically signed and base64-encoded. They’re only secure when they aren’t exposed, so they should be treated like passwords.
* A JWT can be viewed but not manipulated on the client side. You can take your token to jwt.io, choose the algorithm you used to sign, and see the data. You just can’t tamper with it because it’s issued on the server.
* The lifespan of a JWT should be kept short to limit the risk caused by a leaked token.

# When to Use Cookies or Tokens

In general, the choice between a session cookie and a structured token will depend on your use case. You should use cookies when you need to keep track of user interactions, such as with an e-commerce application or website. You can use tokens when building API services or implementing distributed systems.

Another popular option is to use a token inside a cookie. This is by far the most secure option for persisting user sessions, while still giving you all the flexibility you would have with JWT tokens. The technique is simple. You create your token in the backend, but instead of returning the token as a JSON response, you return a cookie that contains the JWT token.

Let’s take a look at how this would look in C# (ASP.NET Core 9.0).

```
public string GenerateToken(string userId, string email)
{
 var tokenHandler = new JwtSecurityTokenHandler();
 var key = Encoding.UTF8.GetBytes(Environment.GetEnvironmentVariable("JWT_KEY")!);

 var claims = new List<Claim>
 {
     new(JwtRegisteredClaimNames.Jti, Guid.NewGuid().ToString()),
     new(JwtRegisteredClaimNames.Sub, userId),
     new(JwtRegisteredClaimNames.Email, email)
 };

 var tokenDescriptor = new SecurityTokenDescriptor
 {
     Subject = new ClaimsIdentity(claims),
     Expires = DateTime.UtcNow.AddMinutes(60),
     Issuer = "https://api.tonit.dev",
     Audience = "https://api.tonit.dev",
     SigningCredentials = new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
 };

 var token = tokenHandler.CreateToken(tokenDescriptor);
 return tokenHandler.WriteToken(token);
}

[HttpPost("login")]
public async Task<ActionResult> Login(LoginDto dto)
{
 var user = await signInManager.UserManager.FindByNameAsync(dto.UserName);

 if (user == null || !await signInManager.UserManager.CheckPasswordAsync(user, dto.Password))
 {
     return Unauthorized();
 }

 var token = tokenGenerator.GenerateToken(user.Id, user.Email!);

 var isDev = Environment.GetEnvironmentVariable("Environment") == "Development";
 Console.WriteLine(isDev);

 // Create cookie options
 var cookieOptions = new CookieOptions
 {
     HttpOnly = true,
     Secure = true,
     SameSite = SameSiteMode.Strict,
     Expires = DateTime.UtcNow.AddHours(1),
 };

 Response.Cookies.Append("jwt", token, cookieOptions);

 return Ok(new { message = "Login successful" });
}
```

As you can see, it is pretty easy. The only thing we have to do is to append the token to the cookies. The rest is more or less the same. What is missing now is to configure the authentication service in the Program.cs file to look for the token inside the cookies.

```
builder.Services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
    .AddJwtBearer(opt =>
    {
        opt.TokenValidationParameters = new TokenValidationParameters
        {
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Environment.GetEnvironmentVariable("JWT_KEY")!)),
            ValidIssuer = "https://api.tonit.dev",
            ValidAudience = "https://api.tonit.dev",
            ValidateIssuerSigningKey = true,
            ValidateLifetime = true,
            ValidateIssuer = true,
            ValidateAudience = true,
        };

        opt.Events = new JwtBearerEvents
        {
            OnMessageReceived = context =>
            {
                if (context.Request.Cookies.TryGetValue("jwt", out var token))
                {
                    context.Token = token;
                }
                return Task.CompletedTask;
            }
        };
    });
```

I won’t go into any more detail on how this works specifically for C# here. You can always read more about it in the official documentation. But to be quite frank with you, once you understand how it works in one language, you should understand it in all other languages as well (at least in theory).

# Conclusion

Great, the trilogy is done! And to be honest, I am quite happy with it. Of course, I can always go more in depth, but I think I hit the sweet spot for people who are not as tech savy.
