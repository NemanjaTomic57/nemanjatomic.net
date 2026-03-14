+++
date = '2026-03-13T15:41:00+01:00'
title = 'How to Build Beautiful Websites With AI'
+++
Websites are the core of every business and public figure. Whatever you are doing, if you want to look credible in front of your customers, you are going to need a website that represents you. And thanks to today's technology, it is easier than ever to build a static website.

In the following article, we will take a look at how we can build a website in the most efficient way possible. The goal is to cut the time commitment while maintaining high quality. You won't need any knowledge at all, since we will build the website completely with AI. In other words, this is a step-by-step tutorial for vibe coding your website without any knowledge of how HTML, CSS, or JavaScript works. So without further ado, let's get started!

# The Tools We Will Use

The first step to our dream website is figuring out what tools we need to use. To be quite frank, this is also the hardest and at the same time most important step we have to take. If we choose the wrong tools, the website won't look good. It's as simple as that. And once we actually have the right tools in place, the rest is a piece of cake. All we have to do is tell the AI exactly what we want to have for our end product and... the AI will do the rest. So, what tools are we going to use?

## OpenAI Codex

There are two major AI models that excel on benchmarks in coding: OpenAI's Codex and Anthropic's Claude Code. Now, most people I spoke with said that Claude Code has a slight edge compared to Codex, especially for coding, but I decided to go with Codex.

The reason is that I like the general responses of ChatGPT more than those from Claude, but I don't have a lot of experience when it comes to Codex. Before I move from Claude, I at least want to give Codex a chance to understand its strengths and weaknesses.

## VSCode

Codex will need a way to interact with our Hugo website, and for that, we need an Integrated Development Environment (IDE). I will choose VSCode because thats my IDE of choice, but you can choose any IDE you want as long as it supports Codex (most of them do).

## Hugo

Next, we want to spice it up a notch. Instead of building "just a static website," let's actually build an application with which it is easy to maintain the content of the website. To make this work, we will use the static site generator [Hugo](
https://gohugo.io/).

To everyone who does not understand what Hugo is, it is essentially similar to WordPress. It lets you build a website with ease because of its simple architecture and wonderful community. You can use different themes, making it super fast to start your blog or documentation. Or you can create your own theme and customize your website to your liking, which is what we are going to do.

However, it does have a few upsides compared to WordPress.

1. Hugo is open-source.

   Yeah, there is not much to say here. Open-source is great, contributing feels even greater, and because it's open-source, you can use it for free. Win-win!

2. Much faster performance.

   Because Hugo generates static HTML files, the pages are pre-built at compile time. We won't need any database queries, and there is no PHP processing on each request. This makes Hugo sites much faster by default.

3. Better security.

   Like I said, Hugo has no database, no login admin panel, and no plugins executing server code. WordPress has everything, and I don't want to think about how many hackers have breached WordPress sites due to a plugin flaw.

4. Lower hosting costs.

   You can host Hugo almost anywhere: GitHub Pages, Cloudflare Pages, Netlify, Vercel, simple object storage... the list goes on and on. The deployment setup takes five minutes.

5. No plugin chaos.

   WordPress often requires many plugins to make the website of your dreams come true. This can become messy in a short time because you start to shove so many plugins into your WordPress application that it turns into a mess. They start to behave not the way they should, and it just feels like... shit.

# Laying the Groundwork

There are some prerequisites we have to check. But because Hugo is such a great product, this won't take too long. It will, however, depend on what operating system you are using.

1. [Install Hugo](https://gohugo.io/installation/)

2. [Install Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

3. Create a new Hugo project with this set of commands:
   ```
   hugo new project quickstart
   cd quickstart
   git init
   hugo server
   ```

Done. That's it. Let's start building.

# Building the Website

Now I want you to open Codex inside your IDE, and I want you to enter a prompt about the look and feel of your website. Be sure to be as specific as you can. The more information the AI receives from you, the more the end product will match your expectations. I have found the best way for me is to write the prompt in a text file first because it's so much easier to edit there. Then copy and paste it to Codex and let the AI do its job.

My prompt looks like the following:

```
Create a new Hugo-based static website for a personal technical blog.

Project goal:

Build a clean, modern, responsive blog aimed at people working in and around IT. The site should feel professional, minimal, and easy to navigate, with strong readability for long-form articles.

Site identity:

- Title: "Nemanja Tomic - Blogging About Tech"
- Subtitle / tagline: "My personal blog where I write about IT and philosophy."
- Single author website
- Language: English

Core requirements:

1. Home page

- Show a short author introduction / hero section.
- Display previews of the latest 3 blog posts.
- Include clear navigation to the full Posts page and About page.

2. Posts page

- List all blog posts.
- Each post supports tags.
- Provide a tag filter on the Posts page so users can filter visible posts by tag.
- Each post preview should include at least:
- title
- publication date
- short summary
- tags
- Clicking a post should open the full post page.

3. About page

- Introduce the author of the blog.
- Include a short personal/professional bio relevant to IT and philosophy.

4. Contact page

- Include a short contact form with the fields:
- Name
- Email
- Message
- The form should be prepared to trigger an email workflow.
- Since Hugo is static, implement the contact form in a production-appropriate way:
- either via a serverless/form backend integration placeholder
- or a form service-compatible HTML form setup
- Clearly isolate the integration points so they can be configured later.

Header / navigation:

- Include these mandatory navigation items in the main header:
- Home
- Posts
- About
- Contact

Design and UX expectations:

- Responsive layout for desktop, tablet, and mobile.
- Clean typography optimized for reading technical articles.
- Accessible semantic HTML.
- Simple, modern styling without heavy visual clutter.
- Support dark mode if practical.
- Keep the layout maintainable and easy to extend.

Technical expectations:

- Use Hugo best practices for content structure, templates, partials, and configuration.
- Organize content into appropriate sections and layouts.
- Create reusable partials for header, footer, and post preview cards.
- Implement taxonomy support for tags.
- Add example content for:
- homepage intro
- about page
- at least 3 sample blog posts with tags
- Make the site easy to run locally with standard Hugo commands.
- Prefer a structure that is easy to deploy as a static site later.

Deliverables:

- Hugo project structure
- layouts, partials, and config files
- styling assets
- sample content
- working tag filtering on the Posts page
- contact page with form markup and clear integration placeholder

Also include:

- a short README explaining:
- project structure
- how to run locally
- how to add new posts
- how to configure the contact form integration

Important:

- Do not overengineer the stack.
- Prioritize simplicity, clarity, and maintainability.
- Generate a polished starter project, not just a bare scaffold.
```

The project starts with a basic Hugo setup. It has a simple structure. This makes it easy to work with. The config file is set up correctly. The site has a homepage, about page, and blog posts. Each blog post has tags. The site is easy to view on a local computer. It can be deployed as a static site later.

The deliverables are ready. They include the project structure, layouts, and config files. The styling assets and sample content are also ready. The Posts page has working tag filtering. The contact page has a form and a clear placeholder for integration.

The README file is short and clear. It explains how the project is set up. It tells users how to run the site locally. It shows them how to add new posts and set up the contact form. The project is simple and easy to maintain. It is a polished starter project, not just a basic setup.

# Conclusion

The website is now ready to use. It has all the basic features we specified in our initial prompt, and we could keep going here to refine every little detail to our wants and needs. The first draft of the website looks great! Granted, it still needs a few tweaks, but it does not look too bad. After those tweaks, the page should be production ready!

As you can see, it is easier than ever to build your own website. You don't even need a lot of time to pull it off. It's a piece of cake. Check out [this website hosted on AWS Amplify](https://main.d1a0bi0o7irp6k.amplifyapp.com/) if you want to see the end result of our initial prompt! Of course there still needs polishing to be done, but it definitively has potential! And you can always follow up with Codex for further fine tuning.
