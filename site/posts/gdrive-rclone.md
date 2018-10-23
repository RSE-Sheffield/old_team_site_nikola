<!--
.. title: Archiving data to Google Drive using Rclone
.. author: Will Furnass
.. slug: gdrive_rclone
.. date: 2018-10-22 17:13:00 UTC
.. tags: google drive rclone storage
.. category:
.. link:
.. description:
.. type: text
-->

At the University of Sheffield (UoS) there are several types of storage available for research data, 

* [Centrally-provided research storage][cics-res-storage]
    * Each [PI][pi] gets 10TB free!
    * Costs £100 per TB beyond that
    * Can be made accessible from [UoS HPC clusters][uos-hpc], from [research VMs][uos-res-vms] and from researchers' own machines
    * Backed up
    * Snapshotted
* Google Drives, provided via [UoS' Google GSuite for Education][uos-gsuite] deal
    * Personal and [Team][google-team-drive] Google Drives
    * Not directly accessible
    * Highly resilient
    * Unlimited storage per user (under this deal)!
* [HPC-specific storage][uos-hpc-storage]
    * Personal areas (10GB + 100GB) are backed up and snapshotted
    * `/fastdata` (Lustre) has a capacity of 667GB and no per-user quotas but is not backed up and data is purged 60 days after it is last modified.
    * None directly mountable on researcher's own machines (other than via e.g. sshfs)

Ideally reseachers would use centrally-provided research storage for most if not all datasets that they want to collaborate on and/or research outputs, but 
for some research groups the free 10TB offering goes further than for others 
so there may occasions when it makes sense to archive intermediary results on Google Drive when 

* The datasets are a non-trivial size
* They needs to be retained for some time
* But don't need to be accessed very frequently

and the research group doesn't have the funds to pay for central IT additional storage 
(which would definitely be preferable if the datasets are *very* large and require further analysis/processing).

So, if under particular circumstances it is necessary to migrate data to/from Google Drive then managing this from the Google Drive web interface would be painful.
What tools are there to help automate transfers?

## Rclone: the Swiss Army Knife of cloud storage

![Rclone logo](/images/gdrive-rclone/rclone-logo.png "Rclone logo")

From the [Rclone][rclone] website: 

> Rclone is a command line program to sync files and directories to and from [a whole bunch of cloud storage services plus more]

(see the Rclone for the full list of supported storage systems).
It draws much inspiration from [rsync][rsync], 
a tool for robustly copying/syncing data between Linux/macOS machines.

So how can we use it to copy data from, say, a UoS HPC system to Google Drive? 

### Install Rclone

*(In the following I assume you're somewhat familiar with using the command-line in Linux/macOS-like environments)*

You can download an executable with no external dependencies 
[from the Rclone website][rclone-download-linux].
(Rclone was written in the [Go][go] programming language, which has the wonderful property of, by default, producing standalone executables upon compilation.)

```sh
# Connect to ShARC
ssh myusername@sharc.shef.ac.uk

# Download Rclone (over HTTPS)
curl -O https://downloads.rclone.org/rclone-current-linux-amd64.zip

# Unarchive Rclone
unzip rclone-current-linux-amd64.zip

# Ensure the program is executable
chmod u+rx ./rclone-*-linux-amd64/rclone

# Create a link to rclone in the bin directory within your home directory
# (which is automatically searched for programs you can run on ShARC,
# so you can run Rclone without needing to specify the path to it)
ln -s ./rclone-*-linux-amd64/rclone ~/bin
```

### Create a Google Service Account

We can't start using Rclone until we provide it with the credentials to allow it to access Google Drive(s).
You *could* configure Rclone to access Google Drive(s) as 'you' (using a fairly secure authentication mechanism called [OAuth][oauth]) 
but then Rclone would be able to read and write all the files you can on Google Drive, 
so a typo might result in you accidentally deleting very large numbers of files or 
copying data to an environment that shouldn't have a copy of it (e.g. for policy reasons).

A better approach could be to create one or more *Service Accounts*.  
These are separate Google Accounts that are (typically) used for automation tasks.
You can then **grant a particular Service Account read-only or read+write access to just a portion of your files**.

To create a Google Project then an associated Service Account:

 1. Log in to the [Google APIs and Services dashboard][goog-api-dash] 
    using your University of Sheffield Google account;
 1. Create a new Project e.g. `rclone-test`

    ![Create a project in Google API dashboard (first step)](/images/gdrive-rclone/goog-api-dash-create-proj-1.png "Create a project in Google API dashboard (first step)")
    ![Create a project in Google API dashboard (second step)](/images/gdrive-rclone/goog-api-dash-create-proj-2.png "Create a project in Google API dashboard (second step)")

    Make a note of the auto-generated unique *Project ID* **(NEEDED?)**
 1. Select your new Project from the dropdown menu at the top of the page.
 1. From the [Credentials part of the dashboard][google-api-dash-creds] click:

     1. *Create credentials* 
     1. *Service Account Key*
     1. Service account: *New service account*
     1. Service account name: *rclone-test-svc-acc*
     1. Role: *Service Accounts* -> *Service Account User*
     1. Key type: *JSON*
     1. *Create*

 1. You will then be prompted to download a file containing the credentials for your new Service Account: **store these securely**.
 1. From the [Identity and Access Management (IAM) part of the dashboard][goog-api-dash-iam] 
    copy the email address associated with the Service User account e.g.
    `rclone-test-svc-acc@rclone-test-220217.iam.gserviceaccount.com`.
    You can also find this address in the `.json` file you downloaded previously.

### Sharing folders with your Service Account

From the Google Drive web interface 
create a new folder for uploading your data and 
share this with that email address.
Ensure that this Service User account can read from the folder (and write to it, if necessary) but 
**cannot change the list of editors / sharing permissions**.

Ideally this folder should be on a Team Drive or 
owned by someone who expects to be at the University for the lifetime of the data.  

### Configure Rclone

**NOTES BELOW**

 8. Create a new rclone *remote* and when prompted say that you want to use a
    service user account and enter the path to the JSON credentials file you
    downloaded.  This works on ShARC or on your local machine.  If you save the
    file on ShARC make sure that it is only readable by you (`chmod 0700
    mycredentials.json`).
 9. Test uploading with rclone using something like (note the use of `--drive-shared-with-me`)

    ```
    rclone copy --verbose --update --progress --drive-shared-with-me somefile.dat uos-gdrive-svc:sharedfolder 
    ```

Perf: 84Mb/s


Checksumming or time+size?

Partial transfers not visible

List files on remote end inc. hashes?

Duplicates?

Throttling - custom `client_id` helps?

Sensitive data



## Command-line

1. Install `google-cloud-sdk` package to get `gcloud` cli
2. Login to Google


    ```
    $ gcloud auth login
    Your browser has been opened to visit:

        https://accounts.google.com/o/oauth2/auth?redirect_uri=....

    WARNING: `gcloud auth login` no longer writes application default credentials.
    If you need to use ADC, see:
      gcloud auth application-default --help

    You are now logged in as [some.gsuite.user@sheffield.ac.uk].
    ```

3. Create project and set enviromnent var to set the current active project for gcloud 

    ```
    $ export CLOUDSDK_CORE_PROJECT=uos-gdrive-svc-gcloud

    $ gcloud projects create $CLOUDSDK_CORE_PROJECT
    Create in progress for [https://cloudresourcemanager.googleapis.com/v1/projects/uos-gdrive-svc-gcloud].
    Waiting for [operations/cp.5682999933913807261] to finish...done. 
    ```

3. Create a Service User account plus credentials and download those credentials as a JSON file:

    ```
    $ SVC_ACC=rclone
    $ gcloud iam service-accounts create $SVC_ACC
    Created service account [rclone].
    $ gcloud iam service-accounts list
    NAME  EMAIL
          rclone@uos-gdrive-svc-gcloud.iam.gserviceaccount.com

    $ gcloud iam service-accounts keys create ${CLOUDSDK_CORE_PROJECT}-${SVC_ACC}-svc-account-creds.json --iam-account=${SVC_ACC}@${CLOUDSDK_CORE_PROJECT}.iam.gserviceaccount.com
    created key [8aaaaaaaaaaa7a0439999999999999999999992d] of type [json] as [uos-gdrive-svc-gcloud-rclone-svc-account-creds.json] for [rclone@uos-gdrive-svc-gcloud.iam.gserviceaccount.com]
    $ ls -ld ${CLOUDSDK_CORE_PROJECT}-${SVC_ACC}-svc-account-creds.json 
    -rw------- 1 will will 2330 Oct  9 21:17 uos-gdrive-svc-gcloud-rclone-svc-account-creds.json
    ```

4. (Optional) Enable the Google Drive API and create an application ID (only needed if want a custom app ID and not to use the default on built in to rclone):

    ```
    $ gcloud services enable drive.googleapis.com
    ```

    HOW TO  CREATE CLIENT ID?


## NOTES: Useful tutorial

http://moo.nac.uci.edu/~hjm/HOWTO-rclone-to-Gdrive.html#_balanced_upload_and_remote_pull_apart

Recursive file listing warning
different globbing behaviour
want to work with larger files (due to high per-file transfer cost)
and select appropriate number of parallel transfers
and 'checkers'

[uos-hpc-storage]: http://docs.hpc.shef.ac.uk/en/latest/hpc/filestore.html
[pi]: https://en.wikipedia.org/wiki/Principal_investigator
[cics-res-storage]: https://www.sheffield.ac.uk/cics/research-storage
[uos-hpc]: http://docs.hpc.shef.ac.uk/en/latest/
[uos-res-vms]: https://www.sheffield.ac.uk/cics/research/infrastructure
[uos-gsuite]: https://www.sheffield.ac.uk/cics/google
[google-team-drive]: https://gsuite.google.com/learning-center/products/drive/get-started-team-drive/#!/
[rclone]: https://rclone.org/
[rsync]: https://rsync.samba.org/
[go]: https://golang.org/
[rclone-download-linux]: https://rclone.org/install/#linux-installation-from-precompiled-binary
[oauth]: https://en.wikipedia.org/wiki/OAuth
[goog-api-dash]: https://console.developers.google.com/ 
[goog-api-dash-creds]: https://console.developers.google.com/apis/credentials
[goog-api-dash-iam]: https://console.developers.google.com/iam-admin/iam
