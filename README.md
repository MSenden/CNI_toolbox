# Computational Neuroimaging Toolbox

The Computational Neuroimaging Toolbox is a MATLAB toolbox for estimating input-referred models. Specifically, the toolbox contains tools for Fourier analyses of phase-encoded stimuli, population receptive field mapping, estimating parameters of generic (user-defined) input-referred models as well as performing ridge regression.

This code is hosted at https://github.com/ccnmaastricht/CNI_toolbox
The latest version may always be found here.

This software was developed with MATLAB R2017a and access to the full suite of MATLAB add-on packages. Some of these packages may be required to run the software.

## Installation
There are two options for installing the toolbox. Either download the toolbox file [Computational Neuroimaging Toolbox.mltbx](https://github.com/ccnmaastricht/CNI_toolbox/raw/master/Computational%20Neuroimaging%20Toolbox.mltbx), navigate to the downloaded file within MATLAB and then execute the following command:

```MATLAB
matlab.addons.toolbox.installToolbox('Computational Neuroimaging Toolbox.mltbx');
```

Alternatively, download the compressed toolbox [Computational Neuroimaging Toolbox.zip](https://github.com/ccnmaastricht/CNI_toolbox/raw/master/Computational%20Neuroimaging%20Toolbox.zip) and extract it into Documents/MATLAB/Add-Ons/Toolboxes.

## Files
This repository contains four files.
1. PEA.m: a MATLAB class implementation of Fourier analysis of phase-encoded stimuli.
2. pRF.m: a MATLAB class implementation of population receptive field mapping.
3. IRM.m: a MATLAB class implementation of input-referred model estimation.
4. RRT.m: a MATLAB class implementation of voxel-wise ridge regression.


### Phase-encoding analysis tool.
pea = PEA(params) creates an instance of the PEA class.
params is a structure with 7 required fields
- f_sampling: sampling frequency (1/TR)
- f_stim    : stimulation frequency
- n_samples : number of samples (volumes)
- n_rows    : number of rows (in-plane resolution)
- n_cols    : number of columns (in-plance resolution)
- n_slices  : number of slices

This class has the following functions

- delay = PEA.get_delay();
- direction = PEA.get_direction();
- PEA.set_delay(delay);
- PEA.set_direction(direction);
- results = PEA.fitting(data);

Use help PEA.function to get more detailed help on any specific
function (e.g. help PEA.fitting)

typical workflow:
1. pea = PEA(params);
2. pea.set_delay(delay);
3. pea.set_direction(direction);
4. results = pea.fitting(data);

### Population receptive field (pRF) mapping tool.
prf = pRF(params) creates an instance of the pRF class.
params is a structure with 7 required fields
  - f_sampling: sampling frequency (1/TR)
  - n_samples : number of samples (volumes)
  - n_rows    : number of rows (in-plane resolution)
  - n_cols    : number of columns (in-plance resolution)
  - n_slices  : number of slices
  - w_stimulus: width of stimulus images in pixels
  - h_stimulus: height of stimulus images in pixels

optional inputs are
  - hrf       : either a column vector containing a single hemodynamic 
                response used for every voxel;
                or a matrix with a unique hemodynamic response along
                its columns for each voxel.
                By default the canonical two-gamma hemodynamic response 
                function is generated internally based on the scan parameters.

This class has the following functions

  - hrf = pRF.get_hrf();
  - stimulus = pRF.get_stimulus();
  - tc = pRF.get_timecourses();
  - pRF.set_hrf(hrf);
  - pRF.set_stimulus(stimulus);
  - pRF.import_stimulus();
  - pRF.create_timecourses();
  - results = pRF.mapping(data);

Use help pRF.function to get more detailed help on any specific function 
(e.g. help pRF.mapping)

typical workflow:
1. prf = pRF(params);
2. prf.import_stimulus();
3. prf.create_timecourses();
4. results = prf.mapping(data);

### Input-referred model (IRM) mapping tool.

irm = IRM(params) creates an instance of the IRM class.
params is a structure with 5 required fields
  - f_sampling: sampling frequency (1/TR)
  - n_samples : number of samples (volumes)
  - n_rows    : number of rows (in-plane resolution)
  - n_cols    : number of columns (in-plance resolution)
  - n_slices  : number of slices

optional inputs are
  - hrf       : either a column vector containing a single hemodynamic 
                response used for every voxel;
                or a matrix with a unique hemodynamic response along
                its columns for each voxel.
                By default the canonical two-gamma hemodynamic response 
                function is generated internally based on the scan parameters.

This class has the following functions

  - hrf = IRM.get_hrf();
  - stimulus = IRM.get_stimulus();
  - tc = IRM.get_timecourses();
  - IRM.set_hrf(hrf);
  - IRM.set_stimulus(stimulus);
  - IRM.create_timecourses();
  - results = IRM.mapping(data);

Use help IRM.function to get more detailed help on any specific function
(e.g. help IRM.mapping)

typical workflow:
1. irm = IRM(params);
2. irm.set_stimulus();
3. irm.create_timecourse(FUN,xdata);
4. results = irm.mapping(data);

### Ridge-based analysis tool.

rrt = RRT(params) creates an instance of the RRT class.
params is a structure with 5 required fields
  - f_sampling: sampling frequency (1/TR)
  - n_samples : number of samples (volumes)
  - n_rows    : number of rows (in-plane resolution)
  - n_cols    : number of columns (in-plance resolution)
  - n_slices  : number of slices

optional inputs are
  - hrf       : either a column vector containing a single hemodynamic
                response used for every voxel;
                or a matrix with a unique hemodynamic response along
                its columns for each voxel.
                By default the canonical two-gamma hemodynamic response
                function is generated internally based on the scan parameters.

This class has the following functions

  - hrf = RRT.get_hrf();
  - X = RRT.get_design();
  - RRT.set_hrf(hrf);
  - RRT.set_design(X);
  - RRT.optimize_lambda(data,range);
  - results = RRT.perform_ridge(data);

Use help RRT.function to get more detailed help on any specific
function (e.g. help RRT.perform_ridge)

typical workflow:
1. rrt = RRT(params);
2. rrt.set_design(X);
3. rrt.optimize_lambda(data,range);
4. results = rrt.perform_ridge(data);