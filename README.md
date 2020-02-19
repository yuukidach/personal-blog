# Personal Blog

This is a personal blog maintained with `hexo`. It is distributed in the GitHub Page.

In my github account, there are 2 repos related to my personal blog: `personal-blog` (this repo) and `yuukidach.github.io`. Their relationship like the source code and the excutable file. I write articles and change the look of the blog in this repo. Then I use hexo to "compile" it and push the result to `yuukidach.github.io`.

## How to build

After swith to a new computer, try the following commands to redistribute the blog:

``` shell
git clone https://github.com/yuukidach/personal-blog
sudo apt install npm
sudo npm install -g hexo
```

If you want to test it

``` shell
hexo s
```

Then the blog will be running at http://localhost:4000
