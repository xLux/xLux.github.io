---
layout: post
title: "octopress使用中文分类"
date: 2015-09-27 11:18:30 +0800
comments: true
categories: 博客搭建
---
在使用octopress博客创建分类列表时，碰到了如果分类名为中文的话，点击之后弹出404页面的问题，下面先列举出网上创建分类列表的基本步骤
然后给出相应问题的解决方法。
<!--more-->
##侧边栏分类的添加
###1、category_list_tag.rb文件<br>
新建plugins/category_list_tag.rb,添加以下内容：<br>
```
module Jekyll
  class CategoryListTag < Liquid::Tag
    def render(context)
      html = ""
      categories = context.registers[:site].categories.keys
      categories.sort.each do |category|
        posts_in_category = context.registers[:site].categories[category].size
        category_dir = context.registers[:site].config['category_dir']
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase)
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
      end
      html
    end
  end
end

Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)
```
以上为Ruby文件的内容,这个插件向liquid注册一个category_list的tag，该tag以li的形式将站点所有的category组织起来。要将category加入到侧边
栏，需要增加一个aside的HTML文件。
###2、增加aside文件
创建source/_includes/asides/category_list.html，并添加以下内容：<br>
![图片](http://7xn37b.com1.z0.glb.clouddn.com/categoriesimage1.png)<br>
注意：如果有中文的话注意保存格式，无BOM，UTF-8
###3、修改_config.yml文件
修改_config.yml文件侧边栏配置选项default_asides,在相应位置将上面创建的HTML文件加上，如下：
```
default_asides: [asides/category_list.html, asides/recent_posts.html]
```
##遇到的问题及解决办法
使用以上方法配置之后，如果文章中得categories中带有中文的话，点击之后会提示找不到页面。这是因为我们在category_list_tag.rb文件中生成的HTML文件的路径不正确，
查看_deploy/blog/categories/目录下，分类的文件夹名字是拼音加横线的形式，而我们并没有进行相应的处理操作，将category_list_tag.rb文件中的
```
html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n"
```
替换为：
```
html << "<li class='category'><a href='/blog/categories/#{category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').to_url.downcase}/'>#{category} (#{posts_in_category})</a></li>\n"
```
即可。