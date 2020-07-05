# slice
Smotastic Lightweight Image Control Envrionment

Application to manage all your local Images in a controlled environment

# Architectural Design
Database (OR JUST METADATA FILE as in JSON???) manages Categories, Folders and Images.

## Image
Image contains the following Attributes
* ID -> technical ID of the entry (auto-generated)
* Name -> The same name as the uploaded image
* Categories 1..n -> A list of category references, to which the image belongs
* Folder n..1 -> The folder reference in which the image resides

## Folder
FOLDER containes the following Attributes
* ID -> technical ID of the entry (auto-generated)
* Name -> The name of the folder
* Google_Folder_Id -> If Google Drive Sync is activated, the FolderId of the Google Drive Folder where this Folder is synchronized to

## Category
CATEGORY containes the following Attributes
* ID -> technical ID of the entry (auto-generated)
* Name -> The name of the category

# Functional Requirements

# Startup

A SOURCE_FOLDER has to be always selected.
This will be the source for all added, deleted, edited images in the Application.

Optionally a Google Drive Sync can be activated. This means that every uploaded image will also be synchronized to the Google Cloud. A folderId has to be provided by the user, where all the synchronizable images will be saved.



# Import
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
The User is able to select images from his local machine, and upload them to the application. 
Once the image is uploaded, the user can select, or create new categories for this image.
Then the user confirms his image, the *Synchronization* Process begins


# Synchronization
Then the user adds, or updates a given image a series of synchronization rules has to be followed:

## Synchronize Physical Copy
The uploaded image is copied to the configured SOURCE_FOLDER, in a subfolder where the name is the birthdate of the image.

## Synchronize FOLDER
A database entry in the table FOLDER exists.
If the FOLDER entry did not exist before, one will be created with the following required columns set:
* **Name** of the folder is the name of the created subfolder


## Synchronize CATEGORY(ies)
Database entries for each category in the table CATEGORY exists.
If new categories there added in the synchronization of the image, every new category will be added to the CATEGORY table.
The required columns for each new category are as follows:
* The **Name** of the category, as the user entered it

## Synchronize IMAGE
A database entry in the table IMAGE exists.
The required columns which are to be filled are as follows:
* The **Name** Column is the same as the synchronized image
* The **Categories** References are set
* The **Folder** Reference, is the reference where the foldername is the same as the birthdate of the image



## TODO Show Images
The user is able to view all previously uploaded images.

The user can filter his view of images:
* By creation Date (listbox)
* By categories (listbox)



# Version Control
Version Control with SVN (only local)
