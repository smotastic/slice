# slice
Smotastic Lightweight Image Control Envrionment

Application to manage all your local Images in a controlled environment.
User can add, delete and edit images and upload them to a local filesystem.
The images can then be categorized and filtered or sorted.

# Architectural Design
There is no need for a database, all data will be managed by metadata files for each folder.
The metadata is a simple JSON file which manages the categories for each image.

For each uploaded image, the image will be placed in a folder, where the name is the birthdate of the image.
For each folder, one metadata file exists in this folder.

The metadata contains two objects.

## Image Categories
The **Image Categories** object contains all images in this folder as a key, and the value is an array of the categories for each image.
``
{
    "image_001": ["Nature", "Animal"],
    "image_002": ["Nature", "Black Forest"],
    "image_003": ["Summerfestival"]
}
``

## Google Drive Sync
The optional **Google Drive Sync Folder ID** is the id of the GDrive Folder where the pictures will be synced to.

``
{"google_drive_sync_folder_id": "abcdef001002"}
``

# Functional Requirements

# Startup

A SOURCE_FOLDER has to be always selected.
This will be the source for all added, deleted, edited images in the Application.

Optionally a Google Drive Sync can be activated. This means that every uploaded image will also be synchronized to the Google Cloud. A folderId has to be provided by the user, where all the synchronizable images will be saved.



# DEPRECATED Import
Create an initial Database out of a given Folder.

The subfolders in the selected Folder always have to follow a predetermined Pattern *{YYYY_MM_DD}_{category...}*

E.g: *2020_05_20_Natur_Tier*

An Import with any amount of subfolders in the given folder will always fill the tables as following

**FOLDER**
For each subfolder in the folder a folder entry in FOLDER will be created.
* Name -> The name of the subfolder

**CATEGORIES**
The folder name includes the names for the categories of this folder.
The categories are determined by using the part after the creation date, and then splitting the string by underscore to retrieve every single category available for this folder.

E.g: *2020_05_20_Natur_Tier*
The part after the creation date is *Natur_Tier*.
This will be splitted by underscore, leaving an array of ["Natur", "Tier"].
The contents of this array are the new categories (or existing categories in case of previous folders) for the CATEGORY table.

**IMAGE**
For each image in each subfolder, an IMAGE entry will be created.
* Name -> The name of the image
* Categories -> The previously created categories for this subfolder (see previous chapter)
* Folder -> The previously created folder reference in where this image resided


# TODO Export
Exports all images in a format, so the images can be viewed without the help of the application. E.g. this is useful for backing up all images in an external hard drive.


# Add Image
The User is able to select images or folders from his local machine, and upload all images to the selected SOURCE_FOLDER. 

If a folder was selected, each image will then recursively be selected for the upload process.

Additionally the user can select, or create new categories for each image, which is about to be uploaded.

Then the user confirms the upload of his image, the *Synchronization* Process begins


# Synchronization
Then the user adds, deletes or updates a given image a series of synchronization rules has to be followed:

## Synchronize Physical Copy


### Add Image
The uploaded image is copied to the configured SOURCE_FOLDER, in a subfolder where the name is the birthdate of the image.

### Delete Image
The uploaded image is removed from the subfolder of the configured SOURCE_FOLDER.

## Synchronize Metadata
A *metadata.json* file exists for each folder.
The metadata contains an object of all synchronized images, where the key is the name of the synchronized image, and the value an array of the selected categories of the user.

### Add Image
If a *metadata.json* does not exist in the folder, a new metadata.json file must be created.
The newly added image, will be appended at the end of the *images* object in the metadata file, with all the selected categories for this image.

### Delete Image
The entry in the *metadata.json* will be removed from the images object.

### Update Image
The categories array of the entry of the edited image will be updated with the newly selected or removed categories of the user.

## Synchronize Globalmetadata
At the root in the SOURCE_FOLDER a *global-metadata.json* file exists.
This file contains the following objects:

* A distinct **categories** array, which contains all globally used categories in each image



## Show Images
The user is able to view all previously uploaded images.

The user can filter his view of images:
* By creation Date (listbox)
* By categories (listbox)


# Version Control
Version Control with SVN (only local)
