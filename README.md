Status: pre-alpha, vercel integration not ready. 

# Quick Wordpress: Roll out a Wordpress site quickly and for free.

The script here will enable to you to quickly start editing a Wordpress site and deploy it for free. This might be handy for you if:

- You need to spin up a simple website quickly and like using Wordpress and associated themes, etc.
- You are OK using GitHub to (publicly or privately) store your website 
- You don't mind signing up for a free account with Vercel

This won't be for you if:

- You want the website to have any dynamic content; this exports the website to a static HTML version - no comments possible!

On the other hand, you can:

- Use a custom domain

## Prerequisites

- [Docker]() and [docker-compose]()
- A GitHub account, and a [Vercel](https://vercel.com) account linked to it. 
- A Vercel [token](https://vercel.com/account/tokens)

## Quickstart

- [Fork]() and clone this repository.
- Have your Vercel [token](https://vercel.com/account/tokens) handy
- Run `./whipit bootstrap`
    - The script will ask you for some bits of information and create the Wordpress instance
    - The site is ready for the initial edits
> - Edit config.yml to fill in a couple of blanks (optional but encouraged).
> - Run quick_wp.py
- Edit the website:
    - Enable the wp2static plugin, edit the settings
    - Click export
- Run `whipit publish`
- Done - the site will be available on the web within a minute or two.
- When finished aditing, shut down the local instance with `whipit stop`

## Where the data is stored

The database is stored on the volume associated with the docker container associated with the site. This isn't backed up. However, `whipit publish` and `whipit backup` export backups of the database, which is, by default, backed up in the repository.

The files associated with the website - image uploads, themes, plugins, etc - are stored in the wp-content directory which again, by default, is stored in the repository. This can be turned off in `config.yml`, however if you lose the docker containers all this content will be lost as well if you are not careful.

The publicly available files are stored in the repository under the `public/` directory.

## Security

The database backups are stored in the repository by default. In theory this poses no security risk, since the Wordpress instance and its database are not actually running anywhere on the public internet; however, if the repository is public this would allow others to clone your website in detail. Either make the repository private, or store the database backups and wp-content directory elsewhere.

## User guide

Run `whipit --help` for more guidance. Useful commands are summarised below:

| command | effect |
|---------|--------|
| `whipit start` | Starts the wordpress for editing |
| `whipit stop` | Shut down the containers |
| `whipit publish` | Commit the changes, kick of publication |
| `whipit backup [file]` | Export the database. |
| `whipit restore [file]` | Imports a database backup |
| `whipit clean` | Removes the containers. |
| `whipit purge` | Delete everything and reset (see below) |
|--------------|-------------------|

## Multiple sites

If you want more than one Wordpress site, there are two options:

- Fork the repository once per website and run `whipit bootstrap` as per above.
- Use one repository, and switch to different branches. 

For the second mode, Vercel support deployment to different domains based on the repository branch. To create a new website beyond the first using this method, do:
```
git checkout -b website2
whipit purge
whipit bootstrap
```
Whipit will detect the branch is not `master` and prompt you with a couple of extra questions. Then edit and publish as normal. Use `git checkout <branch>` to swap between sites. **Note:** If you don't shut down a container while you're still editing that branch, you'll either need to check out that branch and run `whipit stop` or invoke `whipit stop <tag>`.

## Questions, comments and contributions

Please open an issue with any bugs, questions or feedback. Is this useful?

Pull requests welcome with any contributions. Be careful not to make a pull request that commits content from a website; only changes to the scripts are solicited.

Did you get this working on Windows? I'd love to hear about it.
