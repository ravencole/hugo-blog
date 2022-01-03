
---
title: "How to make this blog"
date: 2022-01-03T11:40:01-05:00
---

![Ready to be Published](/images/HowToMakeThisBlog/ready_to_be_published.png)
![Is Published](/images/HowToMakeThisBlog/is_published.png)

Just gonna jot some notes down about how I got this running.

## The Plan
We want to use Hugo to run a static site from Github Pages. This topic is well covered across the internet, but this is how I got mine up and running with minimal configuration.

## Setting Up the Repositories

This setup requires two repositories, the hugo site that we use for development, and the repository that we'll use for hosting the static content.

Navigate to github and create two repositories, one called `blog` (or whatever you would like yours to be called), and the other called `{your_github_username}.github.io`. 
For example, mine is called `ravencole.github.io`.

I've created a new directory in my `~/Projects` folder called `Blog`, enter the directory and we'll init the hugo site.
Locally, this is what our directory structure is going to look like:

```
~/Projects
  |- Blog
    |- blog
    |- ravencole.github.io
```

From inside the `~/Projects/Blog` directory, run:

```bash
hugo new site blog
```

Now we need to connect the hugo site to the `blog` repository.

```bash
cd blog
git init
git remote add origin git@github.com:ravencole/hugo-blog.git
git checkout -b main
git add .
git commit -m "init"
git push -u origin main
```

Back out of the `blog` repository back to `~/Projects/Blog`, and we'll clone the github pages repository:

```bash
git clone git@github:ravencole/ravencole.github.io
git checkout -b main
```

That's all for that repository for the time being. Once we have something to push to the blog we'll come back to it.

## Selecting a theme

We're going to use the [Tale](https://github.com/EmielH/tale-hugo) theme, for ease of use.

Navigate to the `blog` repository and clone the tale theme:

```bash
git clone https://github.com/EmielH/tale-hugo themes/tale
```

Next, edit your `config.toml` file to include: `theme = 'tale'`

This is actually all that we need in order to bring up the hugo development server. From the `blog` directory we'll run:

```bash
hugo server -t tale
```

You should now be able to see your site at [http://localhost:1313](http://localhost:1313).

## Create our first post

We can use the hugo cli tool to create posts:

```bash
hugo new post/MyFirstPost.md
```

There should now be a markdown file at `./content/post/MyFirstPost.md`.

The content should contain only:

```markdown
---
title: "MyFirstPost"
date: 2022-01-03T11:40:01-05:00 // or whatever your current timestamp is
draft: true
---
```

Edit the title parameter to be something like "My First Post", remove the `draft` parameter, and add some content below the configuration:

```markdown
---
title: "My First Post"
date: 2022-01-03T11:40:01-05:00
---

Hello, World.

We're up and running!
```

Your hugo server should have re-compiled and this post should show up on the main page of your development site.

## Generating the static assets

Now that we have a blog post we want to publish, lets compile our assets and push them up to the repo we're hosting our static site on.

Still in the `blog` directory, run:

```bash
hugo -t tale
```

There is now a `public` directory in the root of our project that contains the compiled static assets. 

Now is a good time to commit what we have and push it up to the blog repository.

```bash
git add .
git commit -m "Added first blog post"
git push origin main
```

Next, lets take the compiled assets and add them to the github.io repository:

```bash
cd ../ravencole.github.io
cp -r ../blog/public/* ./
git add .
git commit -m "Added first blog post"
git push origin main
```

## Viewing the site live on GitHub Pages

Pushing to the `main` branch on the github.io repository should trigger GitHub to start building the site. If you navigate to the repository and go to `Settings/Pages`, you should see an alert explaining that your site is ready to be published. You won't find anything at the URL provided there until you see the alert that says "Your site has been published...". This could take a few minutes. Once you see the alert, click the link and you should see the main list page of your hugo site. 

## Wrapping up

Now, the sky is the limit. The first thing I'd recommend doing is adding a custom URL to your blog.
For example, i've set [https://ravencole.com](https://ravencole.com) to redirect to my Hugo blog. 