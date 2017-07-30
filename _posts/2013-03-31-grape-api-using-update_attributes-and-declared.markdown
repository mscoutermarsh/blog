---
layout: post
title: Rails/Grape API - Simplify your PUT methods
date: 2013-03-31 07:35:24.000000000 -07:00
---
<img alt="" src="https://dl.dropbox.com/u/18216283/blog/grape_logo.png" width="344" height="123" />

Updating objects with Grape can be a little tricky and can lead to pretty bloated PUT methods. The simplest way to keep your code clean is to use <a href="http://apidock.com/rails/ActiveRecord/Base/update_attributes">update_attributes</a>. Then you can pass in your params and update your object with just 1 line of code.
```ruby
project.update_attributes(params[:project])
```
The only issue with this is that update_attributes will allow users to update any field that is set as <a href="http://apidock.com/rails/ActiveRecord/Base/attr_accessible/class">attr_accessible</a> in your model.

To limit which fields are exposed through your API, you can use <strong>declared()</strong> to return a hash of only attributes that are in your Grape params block.

Here is a full example of a PUT method to edit a project. Since this is using declared. Only <strong>title</strong> and <strong>description</strong> will be passed into update_attributes regardless of what the user passed to your API. Also notice that I set <strong>include\_missing </strong>to false. This means that if a param is not set, it will not be included in the hash.

```ruby
# Edit a project
desc "Edit a project"
params do
  group :project do
    optional :title, type: String, desc: "Edit title."
    optional :description, type: String, desc: "Edit description."
  end
end
put '/:id' do
  project = Project.find_by_id(params[:id])

  if project.nil?
    error!("Project #{params[:id]} does not exist")
  end

  if project.update_attributes(declared(params[:project], {include_missing: false}))
    project.as_json(only: [ :id, :title, :description])
  else
    error!({error:project.errors.messages})
  end
end
```
