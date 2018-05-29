---
layout: post_page
title: compass与rails4的整合
---

rails的assets pipeline是很方便的东西\
但是默认的sass-rails gem无法使用compass\
compass-rails又不支持rails4\
搜索了很久之后 试出了如下workaround

1.  在Gemfile中加入 `gem "compass-rails", "~> 2.0.alpha.0"`
2.  `bundle install`
3.  config/application.rb中添加
    `config.sass.load_paths << "#{Gem.loaded_specs['compass'].full_gem_path}/frameworks/compass/stylesheets"`
4.  重新启动rails server

完成！ \\ w / 撒花！
