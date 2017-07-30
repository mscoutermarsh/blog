---
layout: post
title: The IO object for media must respond to to_io
date: 2016-07-19 11:05:58.000000000 -07:00
---
If you're getting this error with the `Twitter` gem's `update_with_media` method. And googling around for the solution. I'm here to help you.

```ruby
Twitter::Error::UnacceptableIO
The IO object for media must respond to to_io
```

## Why this happens
When you call `open` in Ruby with a URL. It uses Open::URI to open the file. If the file is under 10kb then it will return a `StringIO` object. If the file is > 10kb, it will return a `File` object.

Under normal circumstances, this is what we want. It's more efficient.

The problem is, the `Twitter` gem, does not handle the case where it is a `StringIO` object. So you get the error `Twitter::Error::UnacceptableIO`.


## Solution
There are a bunch of ways to hack this so it works. Here is what I landed on as the *least hacky* way to fix this issue.

Add this module to your lib folder. This will handle both the normal and `StringIO` cases for you by converting `StringIO`'s to a `File`.

```ruby
# lib/twitter/image.rb
module Twitter::Image
  # The Twitter gem is particular about the type of IO object it
  #   recieves when tweeting an image. If an image is < 10kb, Ruby opens it as a
  #   StringIO object. Which is not supported by the Twitter gem/api.
  #
  #   This method ensures we always have a valid IO object for Twitter.
  def self.open_from_url(image_url)
    image_file = open(image_url)
    return image_file unless image_file.is_a?(StringIO)

    file_name = File.basename(image_url)

    temp_file = Tempfile.new(file_name)
    temp_file.binmode
    temp_file.write(image_file.read)
    temp_file.close

    open(temp_file.path)
  end
end
```

Now, when you're tweeting an image. You can do this.

```ruby
image = Twitter::Image.open_from_url(image_url)
twitter_client.update_with_media("Tweet tweet", image)

```

I hope this helps. If you come across an even better solution, let me know!
