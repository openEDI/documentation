# BUTTER - Empirical Deep Learning Dataset

## Summary
 
This dataset represents an empirical study of the deep learning phenomena on dense fully connected networks, scanning across thirteen datasets, eight network shapes, fourteen depths, twenty-three network sizes (number of trainable parameters), four learning rates, six minibatch sizes, four levels of label noise, and fourteen levels of L1 and L2 regularization each. Multiple repetitions (typically 30, sometimes 10) of each combination of hyperparameters were preformed, and statistics including training and test loss (using a 80% / 20% shuffled train-test split) are recorded at the end of each training epoch. In total, this dataset covers 178 thousand distinct hyperparameter settings ("experiments"), 3.55 million individual training runs (an average of 20 repetitions of each experiments), and a total of 13.3 billion training epochs (three thousand epochs were covered by most runs). Accumulating this dataset consumed 5,448.4 CPU core-years, 17.8 GPU-years, and 111.2 node-years.
 
For each training epoch of each run we recorded 20 performance statistics, and the complete record of these raw values are stored here. For convenience, we additionally include multiple per-experiment summary statistics aggregated over every repetition of that experiment as the "summary" dataset, and also provide several useful slices of the summary data scanning along salient dimensions such as minibatch size and learning rate.
 
## Citing This Dataset
**If you use this dataset, please cite our upcoming dataset publication, which is currently under review for NeurIPS 2022.**

## Additional Resources
+ [Example Jupyter notebooks](https://github.com/NREL/BUTTER-Better-Understanding-of-Training-Topologies-through-Empirical-Results) which plot from the online dataset and produce figures in our publication can be found here: [https://github.com/NREL/BUTTER-Better-Understanding-of-Training-Topologies-through-Empirical-Results](https://github.com/NREL/BUTTER-Better-Understanding-of-Training-Topologies-through-Empirical-Results).
+ The source code for the [empirical machine learning framework](https://github.com/NREL/BUTTER-Empirical-Deep-Learning-Experimental-Framework) used to generate this dataset is available here: [https://github.com/NREL/BUTTER-Empirical-Deep-Learning-Experimental-Framework](https://github.com/NREL/BUTTER-Empirical-Deep-Learning-Experimental-Framework).
+ The [distributed job queue](https://github.com/NREL/E-Queue-HPC) which the framework relies on to distribute the experimental runs over as many systems as desired can be found here: [https://github.com/NREL/E-Queue-HPC](https://github.com/NREL/E-Queue-HPC).


## Description

### Hyperparameter Search Space
 
This is a list of the values of each hyperparameter we swept across in this dataset. **Bold** values are the "core" values that are common to nearly all of the experimental sweeps.
 
+ dataset: Datasets are drawn from the Penn Machine Learning Benchmark Suite (https://epistasislab.github.io/pmlb/)
   + **529_pollen**: regression, 4 features, 1 response variable, 3848 observations
   + **connect_4**: classification, 42 features, 3 classes, 67557 observations
   + **537_houses**: regression, 8 features, 1 response variable, 20640 observations
   + **mnist**: classification, 784 features, 10 classes, 70000 observations
   + **201_pol**: regression, 48 features, 1 response variable, 15000 observations
   + **sleep**: classification, 13 features, 5 classes, 105908 observations
   + **wine_quality_white**: classification, 11 features, 7 classes, 4898 observations
   + nursery: classification, 11 features, 7 classes, 12958 observations
   + adult: classification, 8 features, 4 classes, 4898 observations
   + 505_tecator: regression, 124 features, 1 response variable, 240 observations
   + 294_satellite_image: regression, 36 features, 1 response variable, 6435 observations
   + splice: classification, 60 features, 3 classes, 3188 observations
   + banana: regression, 2 features, 1 response variable, 5300 observations
+ shape: Network shape, which in combination with depth and size determines the neural network topology used.
   + **rectangle**: All layers except for the output layer have the same width.
   + **trapezoid**: Layer widths linearly decrease from input to output.
   + **exponential**: Layer widths exponentially decay from input to output.
   + **wide_first_2x**: Like rectangle, but the first layer is twice as wide as the other layers.
   + rectangle_residual: rectangle but with residual connections for every layer.
   + wide_first_4x: Like rectangle, but the first layer is four times as wide as the other layers.
   + wide_first_8x: Like rectangle, but the first layer is eight times as wide as the other layers.
   + wide_first_16x: Like rectangle, but the first layer is sixteen times as wide as the other layers.
+ size: {**2^5, 2^6, ... 2^24**,  2^25, 2^26, 2^27}
   + The approximate number of trainable parameters in the network. The exact number is recorded in the dataset. If a topology of the desired shape and depth could not be found with a number of trainable parameters within 20% of the size setting, the experiment may not be included. Typically this occurs with only the smallest few size settings. The exact number of trainable parameters is reported in the dataset as num_free_parameters, layer widths are reported in the widths column, and the exact topology is reported in the network_structure column.
+ depth: {**2**,**3**,**4**,**5**,6,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + the number of layers in the network
+ learning rate: {0.01, 0.001, **0.0001**, 0.00001}
+ minibatch size: {32, 64, 128, **256**, 512, 1024}
+ L1 and L2 regularization penalties: {**0.0**, .32, .16, .08, .04, .02, .01, .005, .0025, .00125, .000625, .0003125, .00015625, 7.8125e-5}
+ label noise level: {**0.0**, 0.05, .1, .15, .2}
+ training epochs: {300, **3000**, 30000}
+ repetitions: {10, **30**}
   + The number of times each identical experiment was repeated with different random seeds.
+ optimizer: **ADAM**
+ hidden layer activation function: **ReLU**
+ train-test split: **80% training set, 20% test set**
 
 
 
For regressions, we used mean squared error as the loss function; for binary classifications, we used binary cross entropy; and for all other classifications, we used categorical cross entropy loss and one-hot encoding.
 
### Hyperparameter Sweeps
 
Sweeping the entire hypercube of eight hyperparameter dimensions would require evaluation of over 90 million distinct experiments. To reduce the total number of experiments executed while preserving significant utility of the dataset, we conducted seven overlapping hyperparameter sweeps, each covering a hypercube (or in one case two hypercubes) of the hyperparameter search space:
 
+ primary sweep: The primary sweep tests all datasets and depths with all but the largest sizes without regularization or label noise at the core learning rate and batch size.
   + typical repetitions: **30**
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**, nursery, adult, 505_tecator, 294_satellite_images, splice, banana}
   + shapes: {**rectangle, trapezoid, exponential, wide_first_2x**, rectangle_residual, wide_first_4x, wide_first_8x, wide_first_16x}
   + size: {**2^5, 2^6, ... 2^24**, 2^25}
   + depth: {**2**,**3**,**4**,**5**,6,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + learning rate: **.0001**
   + minibatch size: **256**
   + regularization: **none**
   + label noise: **0**
 
+ 300 epoch sweep: This sweep extends the primary sweep to larger sizes for 300 training epochs.
   + typical repetitions: 10 for rectangle, 0-10 for other shapes
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**}
   + shapes: {**rectangle, trapezoid, exponential, wide_first_2x**}
   + size: {2^25, 2^26, 2^27}
   + depth: {**2**,**3**,**4**,**5**,6,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + learning rate: **.0001**
   + minibatch size: **256**
   + regularization: **none**
   + label noise: **0**
 
+ 30k epoch sweep: This sweep extends the primary sweep to 30 thousand training epochs for smaller sizes.
   + typical repetitions: 10 for rectangle, 0-10 for other shapes
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**}
   + shapes: {**rectangle, trapezoid, exponential, wide_first_2x**}
   + size: {**2^5, 2^6 .. 2^18**}
   + depth: {**2**,**3**,**4**,**5**,6,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + learning rate: **.0001**
   + minibatch size: **256**
   + regularization: **none**
   + label noise: **0**
 
+ learning rate sweep: This sweep scans the core shapes over several learning rates.
   + typical repetitions: **20**
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**, nursery, adult}
   + shapes: {**rectangle, trapezoid, exponential, wide_first_2x**}
   + size: {**2^5, 2^6, ... 2^24**}
   + depth: {**2**,**3**,**4**,**5**,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + learning rate: {0.01, 0.001, **0.0001**, 0.00001}
   + minibatch size: **256**
   + regularization: **none**
   + label noise: **0**
  
 + label noise sweep: This sweep scans the core shapes over several label noise levels at two different learning rates.
   + typical repetitions: **30**
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**}
   + shapes: {**rectangle, trapezoid, exponential, wide_first_2x**}
   + size: {**2^5, 2^6, ... 2^24**}
   + depth: {**2**,**3**,**4**,**5**,**7**,**8**,**9**,**10**, 12, 14, 16, 18, 20}
   + learning rate: {0.001, **0.0001**}
   + minibatch size: **256**
   + regularization: **none**
   + label noise:
     + {**0**, 0.05} at learning rate 0.001
     + {**0.0**, 0.05, .1, .15, .2} at learning rate **0.0001**
 
+ batch size sweep: This sweep scans rectangular networks of the core depths over several batch sizes.
   + typical repetitions: **30**
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**, nursery, adult}
   + shape: **rectangle**
   + size: {**2^5, 2^6, ... 2^24**}
   + depth: {**2**,**3**,**4**,**5**,**7**,**8**,**9**,**10**}
   + learning rate: **0.0001**
   + minibatch size: {32, 64, 128, **256**, 512, 1024}
   + regularization: **none**
   + label noise: **0**
 
+ regularization sweep: This sweep scans rectangular networks of the core depths over several L1 and L2 regularization levels.
   + typical repetitions: **30**
   + datasets: {**529_pollen, connect_4, 537_houses, mnist, 201_pol, sleep, wine_quality_white**, nursery, adult}
   + shape: **rectangle**
   + size: {**2^5, 2^6, ... 2^24**, 2^25}
   + depth: {**2**,**3**,**4**,**5**,**7**,**8**,**9**,**10**}
   + learning rate: **0.0001**
   + minibatch size: **256**
   + regularization:
     + L1: {**0.0**, .32, .16, .08, .04, .02, .01, .005, .0025, .00125, .000625, .0003125, .00015625, 7.8125e-5}
     + L2: {**0.0**, .32, .16, .08, .04, .02, .01, .005, .0025, .00125, .000625, .0003125, .00015625, 7.8125e-5}
   + label noise: **0**
 
 
 
 
If the dataset is made up of multiple files a description of how they are/will
be stored in relation to each other.
 
## Data Format
 
The complete raw dataset is available in the /all_runs/ partitioned parquet dataset. Each row in this dataset is a record of one training run of a network. Several statistics were recorded at the end of each training epoch, and those records are stored in this row as arrays indexed by training epoch. For convenience, we also provide the /complete_summary/ partitioned parquet dataset which contains statistics aggregated over all repetitions of the same experiment including average and median test and training losses at the end of each training epoch. Distinct experiments are uniquely and consistently identified in both datasets by an 'experiment_id'. Additionally, we have created separate raw run and summary datasets for each experimental sweep so that they can be downloaded and queried separately if the entire dataset is not needed. _The schemas of summary and run datasets are the same for every sweep._
 
### File Hierarchy and Descriptions
 
/all_runs/ contains all of the experimental run records for all sweeps
/complete_summary/ contains per-experiment statistics aggregated over every run of each distinct experiment for all sweeps
/complete_summary.tar is a tarball of /experiment_summary/
 
/primary_sweep_summary/ contains summary experiment statistics for the primary sweep
/primary_sweep_summary.tar is a tarball of /primary_sweep_summary/

/300_epoch_sweep_summary/ contains summary experiment statistics for the 300 epoch sweep
/300_epoch_sweep_summary.tar is a tarball of /300_epoch_sweep_summary/
 
/30k_epoch_sweep_summary/ contains summary experiment statistics for the 30k epoch sweep
/30k_epoch_sweep_summary.tar is a tarball of /30k_epoch_sweep_summary/
 
/learning_rate_sweep_summary/ contains summary experiment statistics for the learning rate sweep
/learning_rate_sweep_summary.tar is a tarball of /learning_rate_sweep_summary/
 
/label_noise_sweep_summary/ contains summary experiment statistics for the label noise sweep
/label_noise_sweep_summary.tar is a tarball of /label_noise_sweep_summary/
 
/batch_size_sweep_summary/ contains summary experiment statistics for the batch size sweep
/batch_size_sweep_summary.tar is a tarball of /batch_size_sweep_summary/
 
/regularization_sweep_summary/ contains summary experiment statistics for the regularization sweep
/regularization_sweep_summary.tar is a tarball of /regularization_sweep_summary/
 

 
### Experiment Summary Schema
 
For preliminary analysis, we recommend using the summary dataset as it is smaller and more convenient to work with than the full run dataset. However, the entire record of every repetition of every experiment is stored in the run dataset, allowing other statistics to be computed from the raw data. Each experiment has a unique experiment_id value, which matches run records in the runs dataset. Summary data is partitioned by dataset, shape, learning rate, batch size, kernel regularizer, label noise, depth, and number of training epochs.
 
#### The columns of the summary dataset are:
 
+ experiment_id the unique id for this experiment
+ primary_sweep: bool, true iff this experiment is part of the primary sweep
+ 300_epoch_sweep: bool, true iff this experiment is part of the 300 epoch sweep
+ 30k_epoch_sweep: bool, true iff this experiment is part of the 30k epoch sweep
+ learning_rate_sweep: bool, true iff this experiment is part of the learning rate sweep
+ label_noise_sweep: bool, true iff this experiment is part of the label noise sweep
+ batch_size_sweep: bool, true iff this experiment is part of the batch size sweep
+ regularization_sweep: bool, true iff this experiment is part of the regularization sweep
+ activation: string, the activation function used for hidden layers
+ batch: string, a nickname for the experimental batch this experiment belongs to
+ batch_size: uint32, minibatch size
+ dataset: string, name of the dataset used
+ depth: uint8, number of layers
+ early_stopping: string, early stopping policy
+ epochs: uint32, number of training epochs in this run
+ input_activation: string, input activation function
+ kernel_regularizer: string, null if no regularizer is used
+ kernel_regularizer.l1: float32, L1 regularization penalty coefficient
+ kernel_regularizer.l2: float32, L2 regularization penalty coefficient
+ kernel_regularizer.type: string, name of kernel regularizer used (null if none used)
+ label_noise: float32, amount of label noise applied to dataset before training (.05 means 5% label noise)
+ learning_rate: float32, learning rate used
+ optimizer: string, name of the optimizer used
+ output_activation: string, activation function for output layer
+ shape: string, network shape
+ size: uint64, approximate number of trainable parameters used
+ task: string, name of training task
+ test_split: float32, test split proportion
+ test_split_method: string, test split method
+ num_free_parameters: uint64, exact number of trainable parameters
+ widths: [uint32], list of layer widths used
+ network_structure: string, marshaled json representation of network structure used
+ num_runs: uint8, number of runs aggregated in this summary record
+ num: [uint8], number of runs aggregated in this summary record at each epoch
+ test_loss_num_finite: [uint8], number of finite test losses at each epoch
+ test_loss_avg: [float32], average test loss at each epoch
+ test_loss_stddev: [float32], standard deviation of the test loss at each epoch
+ test_loss_min: [float32], minimum test loss at each epoch
+ test_loss_max: [float32], maximum test loss at each epoch
+ test_loss_median: [float32], median test loss at each epoch
+ train_loss_num_finite: [uint8], number of finite training losses at each epoch
+ train_loss_avg: [float32], average training loss at each epoch
+ train_loss_stddev: [float32], standard deviation of training losses at each epoch
+ train_loss_min: [float32], minimum training loss at each epoch
+ train_loss_max: [float32], maximum training loss at each epoch
+ train_loss_median: [float32], median training loss at each epoch
+ test_accuracy_avg: [float32], average test accuracy at each epoch
+ test_accuracy_stddev: [float32], test accuracy standard deviation accuracy at each epoch
+ test_accuracy_median: [float32], median test accuracy at each epoch
+ train_accuracy_avg: [float32], average training accuracy at each epoch
+ train_accuracy_stddev: [float32], standard deviation of the training accuracy at each epoch
+ train_accuracy_median: [float32], median training accuracy at each epoch
+ test_mean_squared_error_avg: [float32], test MSE at each epoch
+ test_mean_squared_error_stddev: [float32], test MSE standard deviation at each epoch
+ test_mean_squared_error_median: [float32], median test MSE at each epoch
+ train_mean_squared_error_avg: [float32], average training MSE at each epoch
+ train_mean_squared_error_stddev: [float32], training MSE standard deviation at each epoch
+ train_mean_squared_error_median: [float32], median training MSE at each epoch
+ test_kullback_leibler_divergence_avg: [float32], average test KL-Divergence at each epoch
+ test_kullback_leibler_divergence_stddev: [float32], test KL-Divergence standard deviation at each epoch
+ test_kullback_leibler_divergence_median: [float32], median test KL-Divergence at each epoch
+ train_kullback_leibler_divergence_avg: [float32], average training KL-Divergence at each epoch
+ train_kullback_leibler_divergence_stddev: [float32], training KL-Divergence standard deviation at each epoch
+ train_kullback_leibler_divergence_median: [float32], median training KL-Divergence at each epoch
 
### Run Schema
 
Each row of the run dataset represents a single training run. Each training run was instrumented to record various statistics at the end of each training epoch, and those statistics are stored as epoch-indexed arrays in each row. Runs are labeled with an 'experiment_id' which was used to aggregate repetitions of the same experimental parameters together in the summary dataset. An experiment_id in the experiment dataset correspond to the same experiment_id in the summary dataset.
 
#### The columns of the run dataset are:
 
+ experiment_id: uint32, id of the experiment this run was a repetition of
+ run_id: string, unique id of this run
+ primary_sweep: bool, true iff this experiment is part of the primary sweep
+ 300_epoch_sweep: bool, true iff this experiment is part of the 300 epoch sweep
+ 30k_epoch_sweep: bool, true iff this experiment is part of the 30k epoch sweep
+ learning_rate_sweep: bool, true iff this experiment is part of the learning rate sweep
+ label_noise_sweep: bool, true iff this experiment is part of the label noise sweep
+ batch_size_sweep: bool, true iff this experiment is part of the batch size sweep
+ regularization_sweep: bool, true iff this experiment is part of the regularization sweep
+ activation: string, the activation function used for hidden layers
+ batch: string, a nickname for the experimental batch this experiment belongs to
+ batch_size: uint32, minibatch size
+ dataset: string, name of the dataset used
+ depth: uint8, number of layers
+ early_stopping: string, early stopping policy
+ epochs: uint32, number of training epochs in this run
+ input_activation: string, input activation function
+ kernel_regularizer: string, null if no regularizer is used
+ kernel_regularizer.l1: float32, L1 regularization penalty coefficient
+ kernel_regularizer.l2: float32, L2 regularization penalty coefficient
+ kernel_regularizer.type: string, name of kernel regularizer used (null if none used)
+ label_noise: float32, amount of label noise applied to dataset before training (.05 means 5% label noise)
+ learning_rate: float32, learning rate used
+ optimizer: string, name of the optimizer used
+ output_activation: string, activation function for output layer
+ python_version: string, python version used
+ shape: string, network shape
+ size: uint64, approximate number of trainable parameters used
+ task: string, name of training task
+ task_version: uint16, major version of training task
+ tensorflow_version: string, tensorflow version
+ test_split: float32, test split proportion
+ test_split_method: string, test split method
+ num_free_parameters: uint64, exact number of trainable parameters
+ widths: [uint32], list of layer widths used
+ network_structure: string, marshaled json representation of network structure used
+ platform: string, platform run executed on
+ git_hash: string, git hash of version of experimental framework used
+ hostname: string, hostname of system that executed this run
+ seed: int64, random seed used to initialize this run
+ start_time: int64, unix timestamp of start time of this run
+ update_time: int64, unix timestamp of completion time of this run
+ command: string, complete marshaled json representation of this run's settings
+ network_structure: string, marshaled json representation of this run's network configuration
+ widths: uint32, list of layer widths for the network used
+ num_free_parameters: uint64, exact number of free parameters in the tested network
+ val_loss: [float32], test loss at the end of each epoch
+ loss: [float32], training loss at the end of each epoch
+ val_accuracy: [float32], test accuracy at the end of each epoch
+ accuracy: [float32], training accuracy at the end of each epoch
+ val_mean_squared_error: [float32], test MSE at the end of each epoch
+ mean_squared_error: [float32], training MSE at the end of each epoch
+ val_mean_absolute_error: [float32], test MAE at the end of each epoch
+ mean_absolute_error: [float32], training MAE at the end of each epoch
+ val_root_mean_squared_error: [float32], test RMS error at the end of each epoch
+ root_mean_squared_error: [float32], training RMS error at the end of each epoch
+ val_mean_squared_logarithmic_error: [float32], test MSLE at the end of each epoch
+ mean_squared_logarithmic_error: [float32], training MSLE at the end of each epoch
+ val_hinge: [float32], test hinge loss at the end of each epoch
+ hinge: [float32], training hinge loss at the end of each epoch
+ val_squared_hinge: [float32], test squared hinge loss at the end of each epoch
+ squared_hinge: [float32], training squared hinge loss at the end of each epoch
+ val_cosine_similarity: [float32], test cosine similarity at the end of each epoch
+ cosine_similarity: [float32], training cosine similarity at the end of each epoch
+ val_kullback_leibler_divergence: [float32], test KL-divergence at the end of each epoch
+ kullback_leibler_divergence: [float32], training KL-divergence at the end of each epoch
 
 
# This data set and related materials are Licensed Under the Creative Commons Attribution-ShareAlike 4.0 International Licence

Creative Commons Corporation (“Creative Commons”) is not a law firm and does not provide legal services or legal advice. Distribution of Creative Commons public licenses does not create a lawyer-client or other relationship. Creative Commons makes its licenses and related information available on an “as-is” basis. Creative Commons gives no warranties regarding its licenses, any material licensed under their terms and conditions, or any related information. Creative Commons disclaims all liability for damages resulting from their use to the fullest extent possible.

### Using Creative Commons Public Licenses

Creative Commons public licenses provide a standard set of terms and conditions that creators and other rights holders may use to share original works of authorship and other material subject to copyright and certain other rights specified in the public license below. The following considerations are for informational purposes only, are not exhaustive, and do not form part of our licenses.

* __Considerations for licensors:__ Our public licenses are intended for use by those authorized to give the public permission to use material in ways otherwise restricted by copyright and certain other rights. Our licenses are irrevocable. Licensors should read and understand the terms and conditions of the license they choose before applying it. Licensors should also secure all rights necessary before applying our licenses so that the public can reuse the material as expected. Licensors should clearly mark any material not subject to the license. This includes other CC-licensed material, or material used under an exception or limitation to copyright. [More considerations for licensors](http://wiki.creativecommons.org/Considerations_for_licensors_and_licensees#Considerations_for_licensors).

* __Considerations for the public:__ By using one of our public licenses, a licensor grants the public permission to use the licensed material under specified terms and conditions. If the licensor’s permission is not necessary for any reason–for example, because of any applicable exception or limitation to copyright–then that use is not regulated by the license. Our licenses grant only permissions under copyright and certain other rights that a licensor has authority to grant. Use of the licensed material may still be restricted for other reasons, including because others have copyright or other rights in the material. A licensor may make special requests, such as asking that all changes be marked or described. Although not required by our licenses, you are encouraged to respect those requests where reasonable. [More considerations for the public](http://wiki.creativecommons.org/Considerations_for_licensors_and_licensees#Considerations_for_licensees).

## Creative Commons Attribution-ShareAlike 4.0 International Public License

By exercising the Licensed Rights (defined below), You accept and agree to be bound by the terms and conditions of this Creative Commons Attribution-ShareAlike 4.0 International Public License ("Public License"). To the extent this Public License may be interpreted as a contract, You are granted the Licensed Rights in consideration of Your acceptance of these terms and conditions, and the Licensor grants You such rights in consideration of benefits the Licensor receives from making the Licensed Material available under these terms and conditions.

### Section 1 – Definitions.

a. __Adapted Material__ means material subject to Copyright and Similar Rights that is derived from or based upon the Licensed Material and in which the Licensed Material is translated, altered, arranged, transformed, or otherwise modified in a manner requiring permission under the Copyright and Similar Rights held by the Licensor. For purposes of this Public License, where the Licensed Material is a musical work, performance, or sound recording, Adapted Material is always produced where the Licensed Material is synched in timed relation with a moving image.

b. __Adapter's License__ means the license You apply to Your Copyright and Similar Rights in Your contributions to Adapted Material in accordance with the terms and conditions of this Public License.

c. __BY-SA Compatible License__ means a license listed at [creativecommons.org/compatiblelicenses](http://creativecommons.org/compatiblelicenses), approved by Creative Commons as essentially the equivalent of this Public License.

d. __Copyright and Similar Rights__ means copyright and/or similar rights closely related to copyright including, without limitation, performance, broadcast, sound recording, and Sui Generis Database Rights, without regard to how the rights are labeled or categorized. For purposes of this Public License, the rights specified in Section 2(b)(1)-(2) are not Copyright and Similar Rights.

e. __Effective Technological Measures__ means those measures that, in the absence of proper authority, may not be circumvented under laws fulfilling obligations under Article 11 of the WIPO Copyright Treaty adopted on December 20, 1996, and/or similar international agreements.

f. __Exceptions and Limitations__ means fair use, fair dealing, and/or any other exception or limitation to Copyright and Similar Rights that applies to Your use of the Licensed Material.

g. __License Elements__ means the license attributes listed in the name of a Creative Commons Public License. The License Elements of this Public License are Attribution and ShareAlike.

h. __Licensed Material__ means the artistic or literary work, database, or other material to which the Licensor applied this Public License.

i. __Licensed Rights__ means the rights granted to You subject to the terms and conditions of this Public License, which are limited to all Copyright and Similar Rights that apply to Your use of the Licensed Material and that the Licensor has authority to license.

j. __Licensor__ means the individual(s) or entity(ies) granting rights under this Public License.

k. __Share__ means to provide material to the public by any means or process that requires permission under the Licensed Rights, such as reproduction, public display, public performance, distribution, dissemination, communication, or importation, and to make material available to the public including in ways that members of the public may access the material from a place and at a time individually chosen by them.

l. __Sui Generis Database Rights__ means rights other than copyright resulting from Directive 96/9/EC of the European Parliament and of the Council of 11 March 1996 on the legal protection of databases, as amended and/or succeeded, as well as other essentially equivalent rights anywhere in the world.

m. __You__ means the individual or entity exercising the Licensed Rights under this Public License. __Your__ has a corresponding meaning.

### Section 2 – Scope.

a. ___License grant.___

   1. Subject to the terms and conditions of this Public License, the Licensor hereby grants You a worldwide, royalty-free, non-sublicensable, non-exclusive, irrevocable license to exercise the Licensed Rights in the Licensed Material to:

       A. reproduce and Share the Licensed Material, in whole or in part; and

       B. produce, reproduce, and Share Adapted Material.

   2. __Exceptions and Limitations.__ For the avoidance of doubt, where Exceptions and Limitations apply to Your use, this Public License does not apply, and You do not need to comply with its terms and conditions.

   3. __Term.__ The term of this Public License is specified in Section 6(a).

   4. __Media and formats; technical modifications allowed.__ The Licensor authorizes You to exercise the Licensed Rights in all media and formats whether now known or hereafter created, and to make technical modifications necessary to do so. The Licensor waives and/or agrees not to assert any right or authority to forbid You from making technical modifications necessary to exercise the Licensed Rights, including technical modifications necessary to circumvent Effective Technological Measures. For purposes of this Public License, simply making modifications authorized by this Section 2(a)(4) never produces Adapted Material.

   5. __Downstream recipients.__

       A. __Offer from the Licensor – Licensed Material.__ Every recipient of the Licensed Material automatically receives an offer from the Licensor to exercise the Licensed Rights under the terms and conditions of this Public License.

       B. __Additional offer from the Licensor – Adapted Material.__ Every recipient of Adapted Material from You automatically receives an offer from the Licensor to exercise the Licensed Rights in the Adapted Material under the conditions of the Adapter’s License You apply.

       C. __No downstream restrictions.__ You may not offer or impose any additional or different terms or conditions on, or apply any Effective Technological Measures to, the Licensed Material if doing so restricts exercise of the Licensed Rights by any recipient of the Licensed Material.

   6. __No endorsement.__ Nothing in this Public License constitutes or may be construed as permission to assert or imply that You are, or that Your use of the Licensed Material is, connected with, or sponsored, endorsed, or granted official status by, the Licensor or others designated to receive attribution as provided in Section 3(a)(1)(A)(i).

b. ___Other rights.___

   1. Moral rights, such as the right of integrity, are not licensed under this Public License, nor are publicity, privacy, and/or other similar personality rights; however, to the extent possible, the Licensor waives and/or agrees not to assert any such rights held by the Licensor to the limited extent necessary to allow You to exercise the Licensed Rights, but not otherwise.

   2. Patent and trademark rights are not licensed under this Public License.

   3. To the extent possible, the Licensor waives any right to collect royalties from You for the exercise of the Licensed Rights, whether directly or through a collecting society under any voluntary or waivable statutory or compulsory licensing scheme. In all other cases the Licensor expressly reserves any right to collect such royalties.

### Section 3 – License Conditions.

Your exercise of the Licensed Rights is expressly made subject to the following conditions.

a. ___Attribution.___

   1. If You Share the Licensed Material (including in modified form), You must:

       A. retain the following if it is supplied by the Licensor with the Licensed Material:

         i. identification of the creator(s) of the Licensed Material and any others designated to receive attribution, in any reasonable manner requested by the Licensor (including by pseudonym if designated);

         ii. a copyright notice;

         iii. a notice that refers to this Public License;

         iv. a notice that refers to the disclaimer of warranties;

         v. a URI or hyperlink to the Licensed Material to the extent reasonably practicable;

       B. indicate if You modified the Licensed Material and retain an indication of any previous modifications; and

       C. indicate the Licensed Material is licensed under this Public License, and include the text of, or the URI or hyperlink to, this Public License.

   2. You may satisfy the conditions in Section 3(a)(1) in any reasonable manner based on the medium, means, and context in which You Share the Licensed Material. For example, it may be reasonable to satisfy the conditions by providing a URI or hyperlink to a resource that includes the required information.

   3. If requested by the Licensor, You must remove any of the information required by Section 3(a)(1)(A) to the extent reasonably practicable.

b. ___ShareAlike.___

In addition to the conditions in Section 3(a), if You Share Adapted Material You produce, the following conditions also apply.

1. The Adapter’s License You apply must be a Creative Commons license with the same License Elements, this version or later, or a BY-SA Compatible License.

2. You must include the text of, or the URI or hyperlink to, the Adapter's License You apply. You may satisfy this condition in any reasonable manner based on the medium, means, and context in which You Share Adapted Material.

3. You may not offer or impose any additional or different terms or conditions on, or apply any Effective Technological Measures to, Adapted Material that restrict exercise of the rights granted under the Adapter's License You apply.

### Section 4 – Sui Generis Database Rights.

Where the Licensed Rights include Sui Generis Database Rights that apply to Your use of the Licensed Material:

a. for the avoidance of doubt, Section 2(a)(1) grants You the right to extract, reuse, reproduce, and Share all or a substantial portion of the contents of the database;

b. if You include all or a substantial portion of the database contents in a database in which You have Sui Generis Database Rights, then the database in which You have Sui Generis Database Rights (but not its individual contents) is Adapted Material, including for purposes of Section 3(b); and

c. You must comply with the conditions in Section 3(a) if You Share all or a substantial portion of the contents of the database.

For the avoidance of doubt, this Section 4 supplements and does not replace Your obligations under this Public License where the Licensed Rights include other Copyright and Similar Rights.

### Section 5 – Disclaimer of Warranties and Limitation of Liability.

a. __Unless otherwise separately undertaken by the Licensor, to the extent possible, the Licensor offers the Licensed Material as-is and as-available, and makes no representations or warranties of any kind concerning the Licensed Material, whether express, implied, statutory, or other. This includes, without limitation, warranties of title, merchantability, fitness for a particular purpose, non-infringement, absence of latent or other defects, accuracy, or the presence or absence of errors, whether or not known or discoverable. Where disclaimers of warranties are not allowed in full or in part, this disclaimer may not apply to You.__

b. __To the extent possible, in no event will the Licensor be liable to You on any legal theory (including, without limitation, negligence) or otherwise for any direct, special, indirect, incidental, consequential, punitive, exemplary, or other losses, costs, expenses, or damages arising out of this Public License or use of the Licensed Material, even if the Licensor has been advised of the possibility of such losses, costs, expenses, or damages. Where a limitation of liability is not allowed in full or in part, this limitation may not apply to You.__

c. The disclaimer of warranties and limitation of liability provided above shall be interpreted in a manner that, to the extent possible, most closely approximates an absolute disclaimer and waiver of all liability.

### Section 6 – Term and Termination.

a. This Public License applies for the term of the Copyright and Similar Rights licensed here. However, if You fail to comply with this Public License, then Your rights under this Public License terminate automatically.

b. Where Your right to use the Licensed Material has terminated under Section 6(a), it reinstates:

   1. automatically as of the date the violation is cured, provided it is cured within 30 days of Your discovery of the violation; or

   2. upon express reinstatement by the Licensor.

   For the avoidance of doubt, this Section 6(b) does not affect any right the Licensor may have to seek remedies for Your violations of this Public License.

c. For the avoidance of doubt, the Licensor may also offer the Licensed Material under separate terms or conditions or stop distributing the Licensed Material at any time; however, doing so will not terminate this Public License.

d. Sections 1, 5, 6, 7, and 8 survive termination of this Public License.

### Section 7 – Other Terms and Conditions.

a. The Licensor shall not be bound by any additional or different terms or conditions communicated by You unless expressly agreed.

b. Any arrangements, understandings, or agreements regarding the Licensed Material not stated herein are separate from and independent of the terms and conditions of this Public License.

### Section 8 – Interpretation.

a. For the avoidance of doubt, this Public License does not, and shall not be interpreted to, reduce, limit, restrict, or impose conditions on any use of the Licensed Material that could lawfully be made without permission under this Public License.

b. To the extent possible, if any provision of this Public License is deemed unenforceable, it shall be automatically reformed to the minimum extent necessary to make it enforceable. If the provision cannot be reformed, it shall be severed from this Public License without affecting the enforceability of the remaining terms and conditions.

c. No term or condition of this Public License will be waived and no failure to comply consented to unless expressly agreed to by the Licensor.

d. Nothing in this Public License constitutes or may be interpreted as a limitation upon, or waiver of, any privileges and immunities that apply to the Licensor or You, including from the legal processes of any jurisdiction or authority.

> Creative Commons is not a party to its public licenses. Notwithstanding, Creative Commons may elect to apply one of its public licenses to material it publishes and in those instances will be considered the “Licensor.” The text of the Creative Commons public licenses is dedicated to the public domain under the [CC0 Public Domain Dedication](https://creativecommons.org/publicdomain/zero/1.0/legalcode). Except for the limited purpose of indicating that material is shared under a Creative Commons public license or as otherwise permitted by the Creative Commons policies published at [creativecommons.org/policies](http://creativecommons.org/policies), Creative Commons does not authorize the use of the trademark “Creative Commons” or any other trademark or logo of Creative Commons without its prior written consent including, without limitation, in connection with any unauthorized modifications to any of its public licenses or any other arrangements, understandings, or agreements concerning use of licensed material. For the avoidance of doubt, this paragraph does not form part of the public licenses.
>
> Creative Commons may be contacted at creativecommons.org.