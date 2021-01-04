# Reading & Editing DICOM Metadata w/ Python

When my image viewer wouldn’t load a particular CT dataset, I was struck with errors telling me 
that I’m ‘missing geometry information’, looking at the metadata associated with DICOM images 
absolutely overwhelmed me. To those unfamiliar, the number of metadata fields according to the 
official DICOM Standards Committee there are around 3700, nevermind the custom fields that anyone 
else might add! As one lecturer I recall put it:

“DICOM images have the patient’s name, scan date, image resolution, doctor’s name and what the doctor had for breakfast.”

However, some of these are actually required/conditionally required to qualify as a DICOM file and successfully load into a DICOM viewer.

On top of all this, in the 21st century, we’re most likely using tomographic techniques giving us 3 or more dimensional data, since DICOM files only represent a 2D image/slice we end up with several just to make one volume and each one of these files metadata fields must correlate with each other!

### Pydicom

Pydicom is the package you want for all your DICOM metadata field editing (maybe more but I’ve not needed it for anything else). So go ahead and get started with a quick pip install:

```
$ pip install pydicom
```

Like any other python package, we need to import it to use it:

```
>>> import pydicom
```

Loading DICOM dataset (ds):

