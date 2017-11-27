# Younjin Jeong's Blog

본 블로그는 최근 다양하게 사용되고 있는 소프트웨어 엔지니어링 관련 도구의 소개, 테스트등의 목적을 위해 준비 되었습니다. 
블로그는 http://engineering.pivotal.io 에서 hugo 를 기반으로 사용중인 테마 및 도구들을 가져다 사용하고 있으며, 기본적으로 markdown 을 이용해 작성된 포스팅은 TravisCI 를 거쳐 각각 Cloud Foundry 와 github 에 배포되고 있습니다. 




 

production: http://blog.younjinjeong.io

staging: http://blog-staging.younjinjeong.io 

아래는 Pivotal 에서의 좋은글 쓰기에 대한 소개  및 본 블로그의 구동방식에 대해 소개하고 있습니다.
(younjin.jeong@gmail.com, 정윤진)

## Writing a _Good_ Post

**Pair all the time.**  We do everything as a team, and this is no different.  Get feedback from your friends and coworkers.  Show them the post on the staging site, and ask them to tear it apart.

**Keep it technical.**  People want to to be educated and enlightened.  Our audience are engineers, so the way to reach them is through code.  The more code samples, the better.

**Nobody likes a wall of text.**  Use headers to break up your text.  Each image you add to your post increases its XP by 100.  Diagrams, screen shots, or humorous "meme" (_|memā|_) gifs...  They all add color.  If you don't have OmniGraffle, then submit an ask ticket.  There's no excuse for monotony.

**Your 10th grade teacher was right.**  Make use of the hamburger technique.  Your audience doesn't have a lot of time.  Tell them what you're going to write, write it, and then tell them what you've written.  Spend time on your opening.  Make it click.

**Make it pretty.** Pivotal-ui comes with a bunch of nice helpers.  Make use of them.  Check out the example styles in the default post template.

## Post formatting gotchas

Currently, if you have links or line breaks in your `short:` description it will not format correctly.

## Running Locally

This site uses [Hugo](http://gohugo.io) v0.18, which is easy to install:

~~~
$ brew install hugo
$ hugo version
Hugo Static Site Generator v0.18.1 BuildDate: 2016-12-29T09:12:41-08:00
~~~

After cloning this repository, navigate into the new directory, run `bin/watch` in a terminal and then browse to [http://localhost:1313](http://localhost:1313) to see your local copy of the blog.

Hugo has [LiveReload](http://livereload.com/) built in, so if you have that configured in your browser, your window will update as soon as you make a change.  Hugo is *fast*, so you might not realize the reload has already happened.

## Viewing on Staging

You can also double check your work by looking on [the staging site](http://pivotal-cf-blog-staging.cfapps.io/).  Draft posts are only published there.

## Errors???

If you get an error like:

```
Error while rendering page foo: reflect: call of reflect.Value.Interface on zero Value
```

Then you _may_ have forgotten to include a property in your `data/authors/foo.yml` file.

## Publishing Your Copy

Every commit to master is [auto-deployed](https://travis-ci.org/pivotal/blog) to both [production](http://engineering.pivotal.io/) and [staging](http://pivotal-cf-blog-staging.cfapps.io/) (only staging shows drafts).  If you don't have push access, then send an ask ticket to have yourself added to `all-pivots` in this org. To publish your draft post, simply remove the `draft: true` line from the top of your post.

## Changing the style

All of the HTML and CSS live in `themes/pivotal-ui`, which is a port of the [Pivotal UI](https://github.com/pivotal-cf/pivotal-ui) project.  I basically copied the compiled css and image files over.  If you want to change the look of this site, then you should edit the templates in there.

The key files you'll want to look at are:

* `themes/pivotal-ui/layouts/index.html` for the main page...
* `themes/pivotal-ui/layouts/_default/single.html` for the layout around each blog post...
* `themes/pivotal-ui/static/local.css` for CSS changes...
* ...and basically everything else under `themes/pivotal-ui/layouts/`

## Syntax highlighting

[Highlight.js](https://highlightjs.org/) is included for syntax highlighting. Any markdown producing `code pre` tags will be highlighted by default, which means anything like:

<pre><code>~~~ruby
instance = Class.new("foo")
~~~
</code></pre>

If the language auto-detection fails you can add a [language identifier](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). To disable syntax highlighting specify `no-highlight` as the language identifier.


## Under the Hood...

You'll notice that we're not building directly into `public`, but rather into all of `public/local`, `public/prod ` and `public/staging` &mdash; each representing a different environment.  This magic is done by the [bin/build](https://github.com/pivotal/blog/blob/master/bin/build) script.  `cf push` will [push all of the apps](https://github.com/pivotal/blog/blob/master/manifest.yml) one at a time.

We also use:

* [Feedburner](https://feedburner.google.com/fb/a/dashboard?id=lkvb0prnrmdpd4tdcvgd6uorpo) to track RSS subscriptions
* A [twitter account](https://twitter.com/pivotaleng) ([automatically publishes each post](https://feedburner.google.com/fb/a/socialize?id=lkvb0prnrmdpd4tdcvgd6uorpo) via feedburner).
* [Keen.io to collect metrics](https://keen.io/projects/57162c7e59949a7660341912/) 
* [Segment.com for annotating and sending them to Keen.io](https://segment.com/pivotal/sources).
* [PushPop](https://github.com/pushpop-project/pushpop) for sending metrics emails.
* And, of course, [Pivotal Web Services](https://console.run.pivotal.io/organizations/6f501f6a-947d-40e4-b9d8-d36786e85238/spaces/179c6d35-f94d-4226-8b30-83274104aa5c) for hosting ;)

All passwords are stored in Lastpass, available to the CF Engineering Directors, under the "Shared-Blog" folder.
