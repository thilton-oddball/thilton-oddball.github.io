
## Developer Setup

Vets API requires:
- PostgreSQL
    - Including PostGIS
- Redis
- Ruby 2.4.5

### Base Setup

To start, fetch this code:

`git clone https://github.com/department-of-veterans-affairs/vets-api.git`

<div id="clone-vets-api">
<script id="asciicast-RUjdzdl2QoKAtByJy8aFUNvci" src="https://asciinema.org/a/RUjdzdl2QoKAtByJy8aFUNvci.js" data-speed="3" async></script>
</div>

#### Automated (OSX)

If you are developing on OSX, you can run the automated setup script. From
the `vets-api` directory, run `./bin/setup-osx && source ~/.bash_profile && cd
.`

#### Alternative (OSX)

1. Install Ruby 2.4.5
   - It is suggested that you use a Ruby version manager such as
    [rbenv](https://github.com/rbenv/rbenv#installation) and
    [install Ruby 2.4.5](https://github.com/rbenv/rbenv#installing-ruby-versions).
   - *NOTE*: rbenv will also provide additional installation instructions in the
    console output. Make sure to follow those too.
2. Install Bundler to manage dependencies
   - `gem install bundler`
   
<div id="install-bundler">
<script id="asciicast-FvrKFQEn7LPU2uJWEwKs4qWdD" src="https://asciinema.org/a/FvrKFQEn7LPU2uJWEwKs4qWdD.js" data-speed="3" async></script>
</div>

3. Install Postgres and enable on startup
   - `brew install postgres`
   - `brew services start postgres`

<div id="install-postgres">
<script id="asciicast-qP6m9dJO12VUQNCRlPhSp0Gxv" src="https://asciinema.org/a/qP6m9dJO12VUQNCRlPhSp0Gxv.js" data-speed="20" async></script>
</div>

4. Install PostGIS
   - `brew install postgis`

<div id="install-postgis">
<script id="asciicast-CNXY78BJsmWVCu1h7D3U4Y0dI" src="https://asciinema.org/a/CNXY78BJsmWVCu1h7D3U4Y0dI.js" data-speed="30" async></script>
</div>

5. Install Redis
   - `brew install redis`
   - Follow post-install instructions to enable Redis on startup. Otherwise,
    launch it manually with `brew services start redis`.

<div id="install-redis">
<script id="asciicast-UaSV2iPYXBP1YpFV9MrGPJgfp" src="https://asciinema.org/a/UaSV2iPYXBP1YpFV9MrGPJgfp.js" data-speed="3" async></script>
</div>

6. Install ImageMagick
   - `brew install imagemagick`

<div id="install-imagemagick">
<script id="asciicast-wt0PV8fC7u9k8dWnyMc8kvPdz" src="https://asciinema.org/a/wt0PV8fC7u9k8dWnyMc8kvPdz.js" data-speed="15" async></script>
</div>

7. Install Poppler
   -  `brew install poppler`

<div id="install-poppler">
<script id="asciicast-bglBU8sebtYUSHLFEqN0jGfdS" src="https://asciinema.org/a/bglBU8sebtYUSHLFEqN0jGfdS.js" data-speed="15" async></script>
</div>

8. Install ClamAV

  - `brew install clamav`
  - Take note of the the post-install instructions "To finish installation & run clamav you will need to edit the example conf files at `${conf_files_dir}`", which will vary depending on your homebrew installation

    - `cd ${conf_files_dir}` 
    - `touch clamd.sock`
    - `echo "LocalSocket ${conf_files_dir}" > clamd.conf` 
    - `echo "DatabaseMirror database.clamav.net" > freshclam.conf`
    - `freshclam -v`

<div id="install-clamav">
<script id="asciicast-bfO1404XqTKNz6RPSUkUmrY5A" src="https://asciinema.org/a/bfO1404XqTKNz6RPSUkUmrY5A.js" data-speed="15" async></script>
</div>

9. Install [pdftk](https://www.pdflabs.com/tools/pdftk-the-pdf-toolkit/pdftk_server-2.02-mac_osx-10.11-setup.pkg)
10. Install gem dependencies: `cd vets-api; bundle install`

<div id="install-gem-dependencies">
<script id="asciicast-MO9y7K9l05hfIHJydPIJ471kg" src="https://asciinema.org/a/MO9y7K9l05hfIHJydPIJ471kg.js" data-speed="3" async></script>
</div>

   - More information about installing *with* Sidekiq Enterprise as well as our credentials are on the internal system here: https://github.com/department-of-veterans-affairs/vets-api#authentication-required-for-enterprisecontribsyscom

11. Install overcommit `overcommit --install --sign`

<div id="install-overcommit">
<script id="asciicast-VzNNnYm4OkBvWYy3EJvm6bDVs" src="https://asciinema.org/a/VzNNnYm4OkBvWYy3EJvm6bDVs.js" data-speed="3" async></script>
</div>

12. Setup localhost certificates / keys:
   - Create certs directory within config:  `mkdir ./config/certs`
   - Copy the [certificate][certificate] to `./config/certs/vetsgov-localhost.crt`
   - Copy the [key][key] to `./config/certs/vetsgov-localhost.key`
   - *NOTE*: If you don't have access to these keys, running the following
     commands will provide basic functionality, such as for running unit tests:
   - `touch ./config/certs/vetsgov-localhost.crt`
   - `touch ./config/certs/vetsgov-localhost.key`
   
   - *NOTE:* using `touch` to create blank cert and key files means that local authentication with IDme will not work
   
   [certificate]: https://github.com/department-of-veterans-affairs/vets.gov-team/blob/master/Products/Identity/Login/IDme/development-certificates/vetsgov-localhost.crt
   [key]: https://github.com/department-of-veterans-affairs/vets.gov-team/blob/master/Products/Identity/Login/IDme/development-certificates/vetsgov-localhost.key

<div id="setup-localhost-certs-keys">
<script id="asciicast-dReuUt7pIMEjMhZ88QIud0SG4" src="https://asciinema.org/a/dReuUt7pIMEjMhZ88QIud0SG4.js" data-speed="3" async></script>
</div>

13. Create dev database: `bundle exec rake db:setup`
14. Go to the file `config/settings/development.yml` in your local vets-api. Switch the commented out lines pertaining to the cache_dir: uncomment out line 14 (what you use for running the app via Rails), and comment out line 15 (what you use for running the app via Docker).
15. Make sure you have the [vets-api-mockdata](https://github.com/department-of-veterans-affairs/vets-api-mockdata) repo locally installed, preferably in a parallel directory to `vets-api`.
16. Make sure you have the [vets-api-mockdata](https://github.com/department-of-veterans-affairs/vets-api-mockdata) repo locally installed
17. Create a `config/settings.local.yml` file for your local configuration overrides. Add this key pointing to your `vets-api-mockdata` directory. 
```
betamocks:
  cache_dir: ../vets-api-mockdata
```

#### Alternative (Ubuntu 18.04 LTS)
1. Install Postgres, PostGIS, Redis, ImageMagick, Poppler, ClamAV, etc
   - From the `vets-api` directory, run `./bin/install-ubuntu-packages`
1. Edit `/etc/ImageMagick-6/policy.xml` and remove the lines below the comment `<!-- disable ghostscript format types -->`
   - This may not be necessary. The default policy was updated to [fix a variety of vulnerabilities](https://usn.ubuntu.com/3785-1/) as of October, 2018.
1. Install Ruby 2.4.5
   - It is suggested that you use a Ruby version manager such as
    [rbenv](https://github.com/rbenv/rbenv#installation) and
    [install Ruby 2.4.5](https://github.com/rbenv/rbenv#installing-ruby-versions).
   - *NOTE*: rbenv will also provide additional installation instructions in the
    console output. Make sure to follow those too.
1. Install Bundler to manage dependencies
   - `gem install bundler`
1. Install gem dependencies: `cd vets-api; bundle install`
1. Install overcommit `overcommit --install --sign`
1. Setup localhost certificates / keys:
   - Create certs directory within config:  `mkdir ./config/certs`
   - Copy [these certificates](https://github.com/department-of-veterans-affairs/vets.gov-team/tree/master/Products/Identity/Files_From_IDme/development-certificates) into the certs dir.
       - *NOTE*: If you don't have access to these keys, running the following
         commands will provide basic functionality, such as for running unit tests:
       - `touch ./config/certs/vetsgov-localhost.crt`
       - `touch ./config/certs/vetsgov-localhost.key`
1. Create dev database: `bundle exec rake db:setup`
1. Make sure you have the [vets-api-mockdata](https://github.com/department-of-veterans-affairs/vets-api-mockdata) repo locally installed, preferably in a parallel directory to `vets-api`.
1. Create a `config/settings.local.yml` file for your local configuration overrides. Add this key pointing to your `vets-api-mockdata` directory. 
```
betamocks:
  cache_dir: ../vets-api-mockdata
```
