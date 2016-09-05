# Build page

## Core Imports

	fs = require 'fs'

	path = require 'path'


## Library imports

	h = require 'hyperscript'

	marked = require 'marked'

	metah = require 'metah'


## Build page

	google_analytics_script = '''
	(function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
	(i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
	m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
	})(window,document,'script','//www.google-analytics.com/analytics.js','ga');
	ga('create', 'UA-28026518-4', 'auto');
	ga('send', 'pageview');
	'''

	get_page = (content_html_text)->
		meta = metah h

		li_a = (link, text)->
			h 'li', [
				h 'a', href: link, text
			]

		container = h '.container'

		container.innerHTML = content_html_text

		h 'html', lang: 'en', [
			h 'head', [
				meta.charset()

				meta.author 'Matt McKellar-Spence'

				meta.description 'GitHub page of Matt McKellar-Spence.'

				meta.viewport()

				h 'title', 'MattMS on GitHub'

				meta.link_css '/style.css'
			]
			h 'body', [
				h 'main', [
					container
				]

				h 'footer.container', [
					h 'hr'
					h 'ul', [
						li_a 'http://about.mattms.info', 'Read about me'
						li_a 'https://github.com/MattMS/mattms.github.io', 'View on GitHub'
					]
				]

				meta.script google_analytics_script
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
