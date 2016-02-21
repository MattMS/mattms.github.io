# Build page

## Imports

	fs = require 'fs'

	h = require 'hyperscript'

	marked = require 'marked'

	path = require 'path'


## Build page

	get_page = (content_html_text)->
		container = h '.container'

		container.innerHTML = content_html_text

		google_analytics_script = h 'script'

		google_analytics_script.innerHTML = '''
		(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
		(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
		m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
		})(window,document,'script','//www.google-analytics.com/analytics.js','ga');
		ga('create', 'UA-28026518-4', 'auto');
		ga('send', 'pageview');
		'''

		h 'html', lang: 'en', [
			h 'head', [
				h 'meta', charset: 'utf-8'

				h 'meta',
					content: 'Matt McKellar-Spence'
					name: 'author'

				h 'meta',
					content: 'GitHub page of Matt McKellar-Spence.'
					name: 'description'

				h 'meta',
					content: 'width=device-width, initial-scale=1'
					name: 'viewport'

				h 'title', 'MattMS on GitHub'

				h 'link',
					href: 'http://css.mattms.info/basic/light/index.css?t=2014-12-21T1300+10'
					rel: 'stylesheet'
					title: 'Basic light'
					type: 'text/css'

				h 'link',
					href: 'http://css.mattms.info/basic/dark/index.css?t=2014-12-21T1300+10'
					rel: 'stylesheet'
					title: 'Basic dark'
					type: 'text/css'

				h 'link',
					href: 'http://css.mattms.info/solarized/dark/index.css?t=2014-12-21T1300+10'
					rel: 'stylesheet'
					title: 'Solarized dark'
					type: 'text/css'
			]
			h 'body', [
				container

				h 'footer.container', [
					h 'hr'
					h 'ul', [
						h 'li', [
							h 'a', href: 'http://about.mattms.info', 'Read about me'
						]
						h 'li', [
							h 'a', href: 'https://github.com/MattMS/mattms.github.io', 'View on GitHub'
						]
					]
				]

				google_analytics_script
			]
		]

	compile_page = (readme_path, html_path)->
		fs.readFile readme_path, 'utf8', (err, readme_text)->
			if err
				console.error err

			else
				html_dom = get_page marked readme_text

				fs.writeFile html_path, html_dom.outerHTML, (err)->
					if err
						console.error err
					else
						console.log 'Created ./index.html'

	cwd = process.cwd()

	readme_path = path.join cwd, 'README.md'

	html_path = path.join cwd, 'index.html'

	compile_page readme_path, html_path
