# Dark Mode in Phoenix Framework

How to add a button to switch between dark and light mode in Phoenix `>=  1.7.2`.

* Edit `assets/tailwind.config.js`:
  * Add support for manual [dark mode](https://tailwindcss.com/docs/dark-mode#toggling-dark-mode-manually) toggling:
	```js
	module.exports = {
		darkMode: 'class',
		// ...
	```
* Drop `dark_mode.ex` into your `lib/*_web/components` folder.
* Drop `dark_mode.js` into your `js/vendor`.
* Add a hook in `assets/js/app.js`:
  ```js
  import darkModeHook from "../vendor/dark_mode"
  let Hooks = {}
  Hooks.DarkThemeToggle = darkModeHook
	```
	
	Send `Hook` as a `hooks:` object key while creating `liveSocket`:

	```js
	let liveSocket = new LiveSocket("/live", Socket, {params: {_csrf_token: csrfToken}, hooks: Hooks}, )
	```


* Modify your `root.html.heex`:
  * `<html lang="en" class="dark">`
  * Add this as a `<head>`'s first child to run as soon as possible:
	```html
	<script>
		if (localStorage.getItem('theme') === 'dark' || (!('theme' in localStorage) && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
				document.documentElement.classList.add('dark');
		} else {
				document.documentElement.classList.remove('dark')
		}
	</script>
	```

* Drop `<DarkMode.button />` somewhere into your layout.

	![animation](https://github.com/aiwaiwa/phoenix_dark_mode/assets/102391810/b08411b6-6cce-4bd2-9a09-a3adc4254e83)


## This Is Where The Fun Begins

As a starting point, try some body background depending on the dark mode:
* Replace `<body class="bg-white antialiased">` with whatever fits your design:
	* `<body class="text-gray-900 dark:text-white bg-white dark:bg-gray-900 antialiased">`


## Notes

* This a purely JavaScript approach.
* Function component takes advantage of a `phx-update="ignore"` hint to ignore live view updates after initial render.
* We take advantage of a `localStorage` to keep selected theme remembered.
* Feel free to PR.