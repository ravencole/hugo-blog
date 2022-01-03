
---
title: "How to make this blog"
date: 2022-01-03T11:40:01-05:00
summary: "Simple, simple guide to getting a Hugo blog running on GitHub Pages"
cover: "/images/HowToMakeThisBlog/is_published.png"
---

![Published blog on github pages](/images/HowToMakeThisBlog/is_published.png)

**Note**: I've got my [github.io](https://github.com/ravencole/ravencole.github.io) repository hooked up to a domain name, [this](https://dev.to/nickymarino/pointing-a-github-pages-repo-to-a-hover-domain-105e) blog post has an implementation that should work for you.

## The Plan
We want to use [Hugo](https://gohugo.io/) to run a static site from [GitHub Pages](https://pages.github.com/). This topic is well covered across the internet, but this is how I got mine up and running with minimal configuration.

What we'll be putting together is a Hugo site with a repository backed up to GitHub. As well, we're going to create another repository to hook up to our GitHub Page that will host the compiled assets from our Hugo site.
This is going to work fine for me for the time being, but this is a _very_ bare bones implementation. I would not use this for anything besides a personal blog.

## Installing Hugo

Assuming you're on macOS, and that you have [Homebrew](https://brew.sh/) installed, installation is as simple as running:

```bash

	brew install hugo

```

Linux distros should have Hugo in their repositories as well, [Windows folks might find success here](https://wiki.archlinux.org/title/Dual_boot_with_Windows).

**NOTE**: There are two flavors of Hugo currently, Hugo and Hugo Extended. The Extended version comes with processors for SCSS, PostCSS, and many other compilation steps. I'd recommend installing the Extended version if you can. Plenty of themes leverage these build tools, and if you don't have the Extended version you will not be able to use them. However, we do not need the Extended version to get through this tutorial.

## Setting Up the Repositories

This setup requires two repositories, the Hugo site that we'll use for development, and the repository that we'll use for hosting the compiled assets. 

Navigate to GitHub and create two repositories, one called `blog` (or whatever you would like yours to be called), and the other called `{your_github_username}.github.io`. 
For example, mine is `ravencole.github.io`.

I've created a new directory in my `~/Projects` folder called `Blog`.
Locally, this is what our directory structure is going to look like:

```bash

	~/Projects
	  |- Blog
	    |- blog
	    |- ravencole.github.io

```

From inside the `~/Projects/Blog` directory, run:

```bash

	hugo new site blog

```

Now we need to connect the Hugo site to the `blog` repository.

```bash

	cd blog
	git init
	                      # Insert your repository URL below
	git remote add origin git@github.com:ravencole/hugo-blog.git 
	git checkout -b main
	git add .
	git commit -m "init"
	git push -u origin main

```

Back out of the `blog` repository to `~/Projects/Blog`, and we'll clone the GitHub Pages repository:

```bash

	          # Insert your repository URL below
	git clone git@github:ravencole/ravencole.github.io 
	git checkout -b main

```

## Selecting a theme

We're going to use the [Tale](https://github.com/EmielH/tale-hugo) theme, for ease of use.

Navigate to the `blog` repository and clone the Tale theme:

```bash

	git clone https://github.com/EmielH/tale-hugo themes/tale

```

Next, edit your `config.toml` file to include: `theme = 'tale'`

This is actually all that we need in order to bring up the Hugo development server. From the `blog` directory we'll run:

```bash

	hugo server 
	# or
	hugo server -t tale

```

You should now be able to see your blog at [http://localhost:1313](http://localhost:1313).

## Create our first post

We can use the Hugo CLI to create posts:

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

Your hugo server should have re-compiled, and this post should show up on the main page of your development site.

## Generating the static assets

Now that we have a blog post we want to publish, lets compile our assets and push them up to the repository we're hosting our static site on.

Still in the `blog` directory, run:

```bash

	hugo -t tale

```

Hugo has compiled our site to the `public` directory in the root of our project.

Now is a good time to commit what we have and push it up to the blog repository.

```bash

	git add .
	git commit -m "Added first blog post"
	git push origin main

```

In this next section there is a lot of room for automation and improvements. For the time being we'll just do it manually.
We need to move the compiled assets to the GitHub Page repository. 

```bash

	cd ../ravencole.github.io
	cp -r ../blog/public/* ./
	git add .
	git commit -m "Added first blog post"
	git push origin main

```

**NOTE**: Compiling the assets and dumping them into another repository is why this way of running Hugo is fit only for a personal blog. There are a number of traps, but most of them you can get out of by going into the GitHub Page repository, running `rm -rf ./`, and copying your compiled assets back into the folder. It's far from perfect, but is enough to get you up and running. 

## Viewing the site live on GitHub Pages

Pushing to the `main` branch on the `{your_username}.github.io` repository should trigger GitHub to start building the site. If you navigate to the repository and go to `Settings/Pages`, you should see an alert explaining that your site is ready to be published. 

![Screenshot of GitHub saying the site is ready to be published](/images/HowToMakeThisBlog/ready.png)

You won't find anything at the URL provided there until you see the alert that says "Your site has been published...". This could take a few minutes. 

![Screenshot of GitHub saying has been published](/images/HowToMakeThisBlog/is_published.png)

Once you see the alert, click the link and you should see the main list page of your Hugo site. 

## Wrapping up

You're ready to start blogging. I highly recommend reading through the [Hugo Docs](https://gohugo.io/documentation/). You'll likely want to tweak themes, add pages, work with tags, etc. 

I hope you found this useful, thank you for reading :).