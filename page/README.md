# Submarine project website

This readme will walk you through building the Submarine website

## Build website by docker

```
git clone https://github.com/apache/submarine.git
git checkout ghh-pages

docker run -it -p 4000:4000 -v submarine<porject path>:/submarine hadoopsubmarine/submarine-website:1.0.0 sh
cd /submarine
bundle exec jekyll serve --watch --host=0.0.0.0
```

view submarine in local: http://localhost:4000/

## Build website by Local
See https://help.github.com/articles/using-jekyll-with-pages#installing-jekyll

**ruby version:**

    ruby --version >= 1.9.3
    gem install bundler
    bundle install

### Build FAQ

*On OS X 10.9 you may need to do "xcode-select --install"*

Gem *nokogiri* may confilict with `xz` if you have it installed. See https://github.com/sparklemotion/nokogiri/issues/1483
The workaround is to uninstall `zx` before doing `bundle insall`.

## Run website

    bundle exec jekyll serve --watch --host=0.0.0.0


## Deploy to ASF svnpubsub infra (committers only)
 1. generate static website in `./_site`
    ```
    JEKYLL_ENV=production bundle exec jekyll build
    ```

 2. checkout ASF repo
    ```
    svn co https://svn.apache.org/repos/asf/submarine asf-submarine
    ```
 3. copy `submarine/_site/*` to `asf-submarine/site`
 4. ```svn commit -m```

## Adding a new page

    rake page name="new-page.md"
