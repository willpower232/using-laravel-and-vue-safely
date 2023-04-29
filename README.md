# Using Laravel and Vue Safely

Technically this isn't just a Laravel problem, its a using Single File Components (SFCs) in a context where people can enter whatever textual content they like problem.

Vue interpolation has access to functions and when you can access functions, Javascript lets you use the constructor method to execute whatever you please.

Here is [a nice write-up from 2020](https://portswigger.net/research/evading-defences-using-vuejs-script-gadgets).

Using the default UI scaffolding from [laravel/ui](https://github.com/laravel/ui) transforms the majority of the pages content into a Vue app which means anything in `{{ }}` which escapes the Blade rendering will be handled by Vue.

I've included a simple bit of math and popping up the `document.cookie` value which could be easily posted to an endpoint behind the scenes. Thankfully, Laravel defaults the session cookie to httponly which means it isn't accessible by this issue (but if it were then it would be doubly game over) but of course just having a vulnerability means the users could be otherwise affected.

Back in 2018, [Taylors response](https://medium.com/@taylorotwell/js-frameworks-server-side-rendering-and-xss-722805009892) was to add `v-pre` [to the auth scaffolding](https://github.com/laravel/framework/commit/98fdbb098cf52a74441fe949be121c18e3dbbe6a) I haven't used here. Whilst this does fix the problem, I would argue that it isn't a great solution in that it requires you to remember to add it literally anywhere unsanitised user input is in the page and omitting the `v-pre` would require extensive testing to detect.

## More Reading

- https://github.com/laravel/framework/issues/15340
- https://laracasts.com/discuss/channels/laravel/overriding-e-or-any-of-the-laravel-helpers
- https://github.com/vuejs/vue/issues/4223
- https://laracasts.com/discuss/channels/vue/vue-interpolation

(I'm sure there was a discussion in Laracasts about exploiting this within someones Laracasts profile but I can't find it now)
