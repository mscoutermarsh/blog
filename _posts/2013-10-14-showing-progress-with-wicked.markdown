---
layout: post
title: Showing progress with Wicked
date: 2013-10-14 12:03:34.000000000 -07:00
---
![wicked](/assets/archive/images/2014/Jul/Scrumlogs-2013-10-14-20-23-30.jpg)

<a href="https://github.com/schneems/wicked">Wicked</a> is a gem that makes creating a step-by-step wizard really simple.

I was using it on a project recently and wanted to show the user their progress through the wizard.

An easy way to do this would be to show **"Step x of x"**  on the page. Wicked doesn't give you the step numbers, but they are easy enough to grab. Here's how you do it.

In your show action, add the following.

```ruby
@current_step = current_step_index + 1
@total_steps = steps.count
```

Your show action should now look something like this...

```ruby
def show
  @user = current_user
  @current_step = current_step_index + 1
  @total_steps = steps.count
  render_wizard
end
```

Then in your view, you can now easily display your users progress through the wizard.

```ruby
<%= Step #{@current_step} of #{@total_steps} %>
```

Annnd you're done! Your users will now see what step they are on as they use your wizard.
