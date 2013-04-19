---
layout: post
title: Build up octopress style blog site
date: 2012-12-20
comments: true
categories: pragramming
footer: true
---
## Why I choose octopress?
I'm a programmer, and I'm also a lazy guy. One hand, a programmer will encounter quite a lots of questions and problems, such as algorithom making, post reading, open source community contribution, and bug fixing. On the other hand, I'm lazy, a lazy programmer will forget a lots of things, because he is lazy, and too things to memorize, so write them done on the source code will be a good way.

Sometimes I like blogging, just sometimes. In the eariler time, I blog on myspace in Microsoft host website, and later, they decide to shut down the host, and push me transferring my owned stuff to wordpress.com, that is a huge platform, million people blog on that, varios of functionalities even I just use little of them. I'm not a proffsional editor, and I'm living in China, the country where cannot visit facebook, twitter, youtube, and wordpress. But thank godness, we can visit github.com in China.

Always expect there will be a tiny framework, programmer can post blog likes programming. Thanks godness again, octopress can do it...., thank god, thank author, thank all the people who guide me know this.

## My octopress git repository

I follow the official guide to [setup octopress]("http://octopress.org/docs/setup/" "Setup Octopress"), that I can easily install the octopress on my Linux OS. After finish this, I was recommended to build up my blogs on github, since there is programmer's private plots, we can plant anything what we want. And I managment my octopress and draft like below illustrates:

![Alt text](/images/2012-12-20-build-up-octopress-blog/my.octopress.repository.png "My octopress blogs git repository")


Then I follow up the second guideline to [deploy octopress blogs in github]("http://octopress.org/docs/deploying/github/", "deploy on github"). But another problem is comming, during you execute <code>rake setup_github_pages</code>, rake will ask you input your blog repository in <code>git</code> protocal (actually in SSH protocal), that's my office network cannot support, in our office, we can support <code>https</code>. How I can resolve this issue?

## Tips
Open the <code>Rakefile</code> which is under octopress folder, and jump to this code snippet:
{% codeblock lang:ruby %}

task :setup_github_pages, :repo do |t, args|
  if args.repo
    repo_url = args.repo
  else
    puts "Enter the read/write url for your repository"
    puts "(For example, 'git@github.com:your_username/your_username.github.com)"
    repo_url = get_stdin("Repository url: ")
  end
  user = repo_url.match(/:([^\/]+)/)[1]
  branch = (repo_url.match(/\/[\w-]+\.github\.com/).nil?) ? 'gh-pages' : 'master'
  project = (branch == 'gh-pages') ? repo_url.match(/\/([^\.]+)/)[1] : ''

{% endcodeblock %}

You can change the code to support <code>https</code>url, or easilly, hard coding above source code like this:

{% codeblock lang:ruby %}
task :setup_github_pages, :repo do |t, args|
  if args.repo
    repo_url = args.repo
  else
    puts "Enter the read/write url for your repository"
    puts "(For example, 'git@github.com:your_username/your_username.github.com)"
    repo_url = get_stdin("Repository url: ")
  end
  repo_url = "https://github.com/username/blog-repository"
  user = "username" #your github account
  branch = "master" #push to the remote master branch
  project = (branch == 'gh-pages') ? repo_url.match(/\/([^\.]+)/)[1] : ''
{% endcodeblock %}


After that, <code>rake</code> can support <code>https</code>. Others, I only follow the official guideline, now you can see this post means I have successfully built up my blogs, and post blogs in office.

Post blogs like programming, if you are programmer, just do it. Cheers!
