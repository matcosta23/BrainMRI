# Some research on MR Images and brain tumors.

## 2D Dataset with images from different databases.

In addition to not being extensive, databases with MRI brain imaging are restricted in varieties. They either only have healthy scans, or they only have brain tumors with little variety.

The first step was to gather images from different datasets and organize them on a Pandas Dataframe. The images obtained are only samples from the databases to assess the quality of this combination. More images are available in the references.

### Used databases.

1. The Cancer Genome Atlas Glioblastoma Multiforme (TCGA-GBM): [TCGA-GBM - Cancer Imaging Archive](https://wiki.cancerimagingarchive.net/display/Public/TCGA-GBM);
2. The Cancer Genome Atlas Low Grade Glioma (TCGA-LGG): [TCGA-LGG - Cancer Imaging Archive](https://wiki.cancerimagingarchive.net/display/Public/TCGA-LGG);
3. Jun Cheng - Images with three types of cancer: [Brain Tumor Dataset](https://figshare.com/articles/brain_tumor_dataset/1512427);
4. Project with healthy patients: [IXI Dataset](http://brain-development.org/ixi-dataset/);
5. T1 scans from Human Connectome Project available in AWS bucket: [Human Connectome Project](www.humanconnectome.org).

All images were resized to 256x256.

## Relevant notebooks.

#### ***[dcm2nii_brainseg.ipynb](dcm2nii_brainseg.ipynb)***

It converts an image from 'dicom' files to nifti files.

Evaluates the segmentation of an algorithm made available for segmentation.

Some of the images made available at TCGA were used for the BraTS challenge. However, the dataset provided by the competition is a version with segmented brains. The notebook tries to make an equivalence between the three-dimensional scans, despite not being successful due to resizing and normalization.

#### *[hcp_ds.ipynb](hcp_ds.ipynb)*

The same algorithm described above is tested for three-dimensional images of healthy patients from the database **5**. For cancer-free images, the performance is quite satisfactory.

#### *[matlab_ds.ipynb](matlab_ds.ipynb)*

Read images from database **3**. The original files were in *.mat* format. So the code unzips all files, reads it through python, saves it on a dataframe and clears the directory of *.mat* files.

It is important to note that this database has MRI images in coronal, sagittal and axial views. Only images in axial view are used. However, this selection is made on another notebook.

#### *[verify_ds_struct.ipynb](verify_ds_struct.ipynb)*

Performs an analysis of how the datasets are organized and that changes must be made so that it can be possible to bring everyone together on the [create_and_evaluate_2d_ds](create_and_evaluate_2d_ds.ipynb) notebook.

#### *[tcga_tumor_in_brats.ipynb](tcga_tumor_in_brats.ipynb)*

The absence of labeling for each slice of the scans of the TCGA dataset makes its classification possible only by a specialist in the medical field.

However, as some TCGA patients were used by the BraTS dataset, the tumor segmentation provided by the challenge allowed the tumor position to be more accurately found and the 2D images to be labeled correctly.

## Merge and evaluate basic 2D dataset.

The casting of the different images is performed in the [create_and_evaluate_2d_ds.ipynb](create_and_evaluate_2d_ds.ipynb) file.

Images from healthy datasets are selected automatically, while TCGA files were selected manually.

After selecting only axial cuts from the Jun Cheng dataset, a general dataframe is generated.

Within the code, the *EvaluateDataframe* class is responsible for illustrating the requested images, rotating the files (it is desirable that all faces point in the same direction) and even rerotulate any misclassifications.