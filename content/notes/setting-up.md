
### install and configure

1. **quartz**:
	- get [quartz](https://quartz.jzhao.xyz/), and follow the [installation](https://quartz.jzhao.xyz/getting-started/installation) steps. 
		- if using a free github account, our github-repo needs to be `public`, else github won't allow us to emit the website to 'github pages'.
		- we will initialise quartz using the `obsidian` template.
	- create a branch `custom-quartz` branch to track all changes to quartz.
	- customise quartz by editing [configurations](https://quartz.jzhao.xyz/configuration) in `quartz.config.yaml`. 
		- definitely update `pageTitle`
		- optional updates:
			- `analytics: null`
			- `locale: en-GB`
2. **obsidian**:
	- note: track content content-changes in a separate `update-content` branch (within the repo).
	- download [obsidian](https://obsidian.md/) and open it.
	- open the `content` folder as an obsidian-vault.
		- this will create a `.obsidian` folder inside our `garden/obsidian` folder; and the vault will open in obsidian.
	- customise obsidian vault's preferences:
		- make folders
		- set locations for:
			- page templates: `_templates` 
			- attachments (images, files, etc): `_attachments`
			- daily-notes: `log`
			- regular notes: `notes`
		- create the folders: `_templates`, `_attachments`, `log` and `notes`
		- for daily-notes,
			- set date format to `custom`, and change the syntax from `YYYY-MM-DD` to `YYYYMMDD`
		- in `file recovery`
			- increase `history length` (eg. 1000)
		- in `core plugins`, 
			- enable `slash commands`
			- enable `daily notes`
		- (optional) enable `community plugins`, and, and install plugins:
				- ~~[image converter](obsidian://show-plugin?id=image-converter)~~
				- ~~[update modified date](obsidian://show-plugin?id=frontmatter-modified-date)~~
				- ~~[tag page](https://community.obsidian.md/plugins/tag-page-md)~~
		- under `files & links`, set deleted files to move to obsidian's `.trash` folder (to make them easier to recover in-case of accidental deletion).
			- add `.trash` to `.gitignore`

### write.

at this point, this is good to know: 
- most files in the obsidian folder will be `.md` files. 
- while obsidian is a convenient tool for writing and managing your content, these files can be opened/edited using any text-editor. 
- however, if we plan to use quartz to emit the files to a website — which, we do, because it's easy for students to pick this up and start writing-in-public *quickly* — , then it is best to use obsidian because (among other reasons) quartz is optimised for the *flavour of markdown* that obsidian uses.

instructions for writing:
- make sure that you're in the `update-content` branch.
- the vault is open in obsidian. so, *write!*
	- read about basic [markdown syntax](https://quartz.jzhao.xyz/getting-started/authoring-content#syntax) you can use while creating notes. 
	- i suggest updating  `index.md`. (when we _emit_ the website, quartz will turn this into your website's home page.) 
- from time to time, commit changes to git.

tips:
- you can copy over existing files (from an existing obsidian vault, or just any folder in which you had notes in a suitable format). paste those items into appropriate sub-folders (eg. `log`, `notes`, etc).
- ==do not add any private items.== while notes marked as `draft: true` or `publish: false` may—depending on the plugins you use—not get emitted to the final website, **all your content** (and every version of it) will still sit among the commits in a public git repository.
- about *tags*: 
	- unlike logseq, obsidian doesn't offer pages for tags. also: it is ~~not possible~~ *very inconvenient* to rename a tag (say, change `#ideas` to `#thoughts`). so, i don't like using tags (or have to be very disciplined if i must use them). 

### emit locally

- serve (preview) the website: 
	- in the terminal, type: `npx quartz build --serve`. 
	- the site will be live on  [http://localhost:8080](http://localhost:8080)
- to speed up build/serve times, the [custom og images](https://quartz.jzhao.xyz/plugins/CustomOgImages) plugin can be disabled in the `quartz.config.ts` file (under `plugins` > `emitters`).

### emit to github pages

(note: regularly commit and push changes to github.)

- locally:
	- 'missing dependency' fix
		- when github pages tries to build the website, it may run into an error (`Cannot find module '@quartz-themes/default'`). to preemptively fix this, go to the local `quartz` folder,
			- open your terminal in the `quartz` folder.
			- Run: `npm install @quartz-themes/default`
			- This will update your `package.json` and `package-lock.json` files.
	- create a `emit` branch. 
		- whenever changes are made,
			- commit changes made to quartz into `custom-quartz`
			- commit all content updates into `update-content`
			- and then merge the updated branch into `emit`.
	- setup github-actions
		- create `deploy.yml`. [instructions](https://quartz.jzhao.xyz/hosting#github-pages).
		- update `deploy.yml`: 
			- since we want the website to auto-update on any update (whether to quartz or content), change the default `v5` branch to `emit`.
- on github: in the repo's `settings`, 
	- (optional) change the default branch from `v5` to `emit`.
	- under `pages`, ensure that:
		- github pages are enabled.
		- source is `github actions`. // there's no need to select any workflow.
	- under `environments`, go into the `github-pages` environment, and change the allowed branch from `v5` to `emit`
- locally: merge changed branches into `emit`; push. on github: check github actions. if it completes successfully, the website will be emitted to github-pages.