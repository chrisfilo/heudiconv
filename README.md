# HeuDiConv

This is a flexible dicom converter for organizing brain imaging data into
structured directory layouts.

- it allows flexible directory layouts and naming schemes through
  customizable heuristics implementations
- it only converts the necessary dicoms, not everything in a directory
- you can keep links to dicom files in the participant layout
- it's faster than parsesdicomdir or mri_convert if you use dcm2nii option
- it tracks the provenance of the conversion from dicom to nifti in w3c
  prov format
- the example shows a conversion to openfmri layout structure

## How it works (in some more detail)

Call heudiconv like this:

    heudiconv -d '%s*.tar*' -s xx05 -f ~/myheuristics/convertall.py

where `-d '%s*tar*'` is an expression used to find DICOM files (`%s` expands to
a subject ID so that the expression will match any `.tar` files, compressed
or not that start with the subject ID in their name). `-s od05` specifies a
subject ID for the conversion (this could be a list of multiple IDs), and
`-f ~/myheuristics/convertall.py` identifies a heuristic implementation for this
conversion (see below) for details.

This call will located the DICOMs (in any number of matching tarballs), extract
them to a temporary directory, search for any DICOM series it can find, and
attempts a conversion storing output in the current directory. The output
directory will contain a subdirectory per subject, which in turn contains an
`info` directory with a full protocol of detected DICOM series, and how their
are converted.

### The `info` directory

The `info` directory contains a copy of the heuristic script as well as the
dicomseries information. In addition there are two files NAME.auto.txt and
NAME.edit.txt. You can change series number assignments in NAME.edit.txt and
rerun the converter to apply the changes. To start from scratch remove the
participant directory.  

## Outlook

soon you'll be able to:
- add more tags to the metadata representation of the files
- and push the metadata to a provenance store
