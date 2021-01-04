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

```
>>> ds = pydicom.filereader.dcmread(filepath)
```

Printing all metadata: (example)

```
>>> print(ds)
(0008, 0008) Image Type                          CS: ['DERIVED', 'SECONDARY']
(0008, 0015) Instance Coercion DateTime          DT: ''
(0008, 0016) SOP Class UID                       UI: Secondary Capture Image Storage
(0008, 0020) Study Date                          DA: '20190405'
(0008, 0030) Study Time                          TM: '150613'
(0008, 0050) Accession Number                    SH: ''
(0008, 0060) Modality                            CS: ''
(0008, 0070) Manufacturer                        LO: ''
(0008, 0090) Referring Physician's Name          PN: "Referring Physician's Name"
(0008, 1030) Study Description                   LO: ''
(0008, 103e) Series Description                  LO: 'CT'
(0010, 0010) Patient's Name                      PN: 'Name'
(0010, 0020) Patient ID                          LO: 'ID'
(0010, 0030) Patient's Birth Date                DA: ''
(0010, 0040) Patient's Sex                       CS: ''
(0018, 0050) Slice Thickness                     DS: "1.000000"
(0018, 0088) Spacing Between Slices              DS: "1.000000"
(0018, 1030) Protocol Name                       LO: ''
(0020, 0010) Study ID                            SH: ''
(0020, 0011) Series Number                       IS: ''
(0020, 0032) Image Position (Patient)            DS: ['0.000000', '0.000000', '0.000000']
...
```

Viewing individual fields & setting fields to new values:

```
# Viewing Field
>>> print(ds.PatientName)
'Name'
# Setting field to new value
>>> ds.PatientName = 'NewName'
>>> print(ds.PatientName)
'NewName'
```

### Dicom Tags

It may have been noted that every field has an individual code (0000, 0000), known as a tag. The first four numbers denote the field’s group (e.g. 0010 = Patient) and 2nd four denote the ‘element’. The individual numbers of the 4 numbered tags are hexadecimal so instead of taking on values from 0–9 to mean 0–9, they can extend beyond to take on values up to 15 by including the first 6 letters of the alphabet. You can look up these standard tags.

We can use these tags instead of the name to change fields where `0x` in python denotes the conversion of a value to hexadecimal.

```
# Viewing Tag
>>> print(ds[0x10,0x10])
'Name'
# Setting tag to new value
>>> ds[0x10,0x10].value = 'NewName'
>>> print(ds[0x100010])
'NewName'
```

Note the different ways that hexadecimal reference to the tag can be made.

