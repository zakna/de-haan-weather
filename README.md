# de-haan-weather

De Haan weekend planner: a single page, `index.html`.

## Deploy

`.github/workflows/deploy.yml` runs on every push to `main` and on manual dispatch (Actions > deploy > Run workflow; it defaults to a dry run that only lists changes, untick `dry_run` to upload). It copies `index.html` to the `dehaan/` folder of the OVH FTP home over SFTP with `lftp mirror --delete`, so the site is served at https://dehaan.zakna.be.

Safety: the upload only ever writes inside `dehaan/`. It stops if that folder does not exist and never touches `www/` (the CV site) or any other folder. The SFTP host key is pinned in `.github/ovh_known_hosts`.

## One-time setup (owner)

1. In OVH Multisite, add the domain `dehaan.zakna.be` with root folder `dehaan` (this creates the folder) and enable SSL.
2. In this repo, under Settings > Secrets and variables > Actions, add:
   - variable `OVH_SFTP_HOST`: the FTP host, `ftp.cluster129.hosting.ovh.net`
   - secret `OVH_SFTP_USER`: the FTP user
   - secret `OVH_SFTP_PASSWORD`: its password
3. Run the workflow once and check https://dehaan.zakna.be. Until step 2 is done the run fails with a message naming what is missing.
4. After that first successful deploy, disable GitHub Pages (Settings > Pages) so GitHub no longer hosts the site.
