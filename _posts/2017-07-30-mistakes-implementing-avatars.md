---
layout: post
title: Mistakes implementing avatars
---

In the past 3 years, we've gone through several iterations of how we host
avatars on [Product Hunt](https://www.producthunt.com).

There were a lot of mistakes.

This is what I recommend doing if you're adding avatars to your web app.

**Short version:**
```
1. Upload the original image directly to S3.
2. Use deterministic filenames/urls. No database queries for an avatar.
3. Use a service for transforming images on READ. Put imgix infront of S3.
```

## More info

### Transform on read, not on write

Upload the original image directly to S3. Do not transform/resize. It's
impossible to know all of the sizes you'll need for an avatar. People care a lot
about what their avatar looks like, if you make changes in quality or display it
using the wrong resolution, you'll hear about it.

It's impossible to predict what size you'll need for every use case or the latest retina screen.

When getting this wrong, you have to go back and resize all existing
avatars. If you have a lot of users, this process can take days.

Use another service to transform/resize the avatar. We use [imgix](https://imgix.com).
Imgix allows you to add params to transform the image on read. It's then stored in their CDN.
(if cost is an issue, there are less powerful, open source, roll-your-own solutions for this available)

### Deterministic URL

This keeps things simple. You'll never have to make a database query to get an
avatar. Your frontend can assume what the URL is.

You can do this with a user_id, such as `/:user_id/avatar`.

Or if you want your URLs to be more difficult to guess. Copy [gravatars approach with a hash](https://en.gravatar.com/site/implement/images/).

If you're using a frontend framework like React, rendering an avatar becomes
very easy. All that's needed is the `user_id`.

```javascript
<UserImage user_id=5 width="30px" height="30px" />
```
