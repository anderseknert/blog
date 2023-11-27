Title: Quickblog
Date: 2023-11-27
Tags: clojure,meta,quickblog

While the [bronchitis](bronchitis) is still keeping me from working for any longer periods of time — and two kids claiming much of the little time I've got to spare — I've found at least a few free moments for getting started on this blog. A few attempts to run a blog have been made in the past, but most often I've gotten stuck in the technical details of the blog platform itself. While I do like to tinker, it's an easy trap to fall into, spending way too much time getting things just the way you want them, and then grow tired of it all before you even get to the actual writing. Perfect is the enemy of good, and all that.

To avoid that this time around, I went with [Quickblog](https://github.com/borkdude/quickblog/), which is a static site generator written in Clojure. The project is maintained by Michiel Borkent, a.k.a. [@borkdude](https://github.com/borkdude) who is one of the most prolific contributors to the Clojure ecosystem, and who I've had some great interactions with during my intermittent Clojure stints. Michiel is also the creator of [clj-kondo](https://github.com/clj-kondo/clj-kondo), which was an inspiration for me when building [Regal](https://github.com/styraInc/regal).

Quickblog strikes a good balance between simplicity and flexibility, much thanks to its Clojure foundation. I also love the fact that there's only a dozen or so blogs using it yet, most (?) of which are listed in the README for the project. Judging by the commit history of quickblog, many of those authors have also contributed something to the project. Grassroots community vibes!

### To tinker or not to tinker

The out-of-the-box experience is decent, and while I'll want to tweak the theme more than I've done so far, it doesn't feel too urgent. There was really only one thing I wanted to fix right away — the `.html` extension used for links throughout the site. It's a vanity thing for sure, but I've always prefered my URLs free from those, so let's try and fix that!

Most web servers will automatically serve e.g. `/foo.html` when `/foo` is requested, and GitHub Pages is no exception. The [http-server](https://github.com/babashka/http-server) embedded in quickblog and used for local testing however did not, so first order of operations was a small contribution to [allow that](https://github.com/babashka/http-server/pull/10).

With that out of the way, the next step was to make the `.html` extension optional in quickblog's renderer. [Adding](https://github.com/borkdude/quickblog/pull/79) a configurable `:page-suffix` option took a little more work, but was a great introduction to the codebase. Both of my PRs got merged within an hour, which is just phenomenal! Hopefully I've now got enough to keep me from tinkering for a while, but we'll see...

If you're curious about what the code for my blog looks like — and intentionally there isn't much of it! — it's available [on GitHub](https://github.com/anderseknert/blog).

### Publishing to GitHub Pages

The final step was remarkably easy. Following the steps outlined in the GitHub docs for [Publishing with a custom GitHub Actions workflow]( https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow), I was able to render and publish the blog in just a few minutes. The workflow snippet below should be fairly self-explanatory, but to summarize:

1. Checkout the repo
2. Get [Babashka](https://github.com/babashka/babashka)
3. Render the blog
4. Upload the `public/` directory as a build artifact
5. Deploy the artifact to GitHub Pages

GitHub recommended running the deployment as a separate job, so that's what I went with.

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: delaguardo/setup-clojure@12.1
        with:
          bb: latest
      - run: bb quickblog render
      - uses: actions/upload-pages-artifact@v2
        with:
          path: public/
  deploy:
    needs: build
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/deploy-pages@v2
        id: deployment
```

Could be that caching could speed up the process in case I ever have hundreds of posts at some point, but let's worry about that later. One thing I'm noticing now though is the missing syntax highlighting for the YAML block above. Unsurprisingly, that works for Clojure, but I've not tried other languages. Will definitely want support for [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/) too later. Oh well, just to tinker a _little_ more...

**Update: 2023-11-27**
Syntax highlighting fixed. Turned out to be rather simple to simply pick whatever languages one would [want](https://prismjs.com/download.html#themes=prism&languages=markup+css+clike+javascript). Also now included in local distribution rather than fetched from CDN.
