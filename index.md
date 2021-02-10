<br>
<br>
# Sex Differences in Functional Topography of Association Networks
*Prior work has shown that there is substantial interindividual variation in the spatial distribution of functional networks across the cerebral cortex, or “functional topography”. However, it remains unknown whether there are normative developmental sex differences in the topography of individualized networks. Here we leverage regularized non-negative matrix factorization to define individualized functional networks in 693 youth ages 8-23y imaged with fMRI as part of the Philadelphia Neurodevelopmental Cohort. Support vector machines were applied to examine sex differences in multivariate patterns of functional topography. Generalized additive models with penalized splines were then used to examine the impact of sex on topography at a more granular level. We used chromosomal enrichment analyses to assess the correlation between gene expression (Allen Human Brain Atlas) and sex differences in functional topography, evaluating significance with gene-wise non-parametric permutation tests. This project identifies normative developmental sex differences in the functional topography of association networks and highlight the role of sex as a biological variable in shaping functional brain development in youth.*

### Project Lead
Sheila Shanmugan

### Faculty Leads
Theodore D. Satterthwaite  
Aaron Alexander-Bloch

### Analytic Replicator
Zaixu Cui

### Collaborators 
Jakob Seidlitz, Zaixu Cui, Azeez Adebimpe, Danielle S. Bassett, Maxwell A. Bertolero, Christos Davatzikos, Damien A. Fair, Raquel E. Gur, Ruben C. Gur, Hongming Li, Adam Pines, Armin Raznahan, David R. Roalf, Russell T. Shinohara, Jacob Vogel, Daniel H. Wolf, Yong Fan

### Project Start Date
January 2019

### Current Project Status
In preparation

### Datasets
?

### Github Repository
<https://github.com/sheilashanmugan/funcParcelSexDiff>

### Path to Data on Filesystem 
/cbica/projects/funcParcelSexDiff/data

### Publication DOI


### Conference Presentations


<br>
<br>
# CODE DOCUMENTATION  
The steps below detail how to replicate this project, including statistical analysis and figure generation.  

### Atlas Generation and Network Parcellation  
1. Sample selection, atlas generation, and individual network parcellation.  

    > These steps were completed as part of prior work (Cui et al., 2020) using scripts located here: https://github.com/ZaixuCui/pncSingleFuncParcel . Loading matrices for each of the 693 subjects used in this project can be found here: /cbica/projects/funcParcelSexDiff/data/Revision/SingleParcellation/SingleAtlas_Analysis/FinalAtlasLoading . The following steps use this preprocessed data.  

### Multivariate Pattern Analysis  
1. Add [/matlabFunctions/Toolbox/Code_mvNMF_l21_ard_v3_release/](https://github.com/sheilashanmugan/funcParcelSexDiff/tree/gh-pages/matlabFunctions/Toolbox/Code_mvNMF_l21_ard_v3_release) to matlab path.  

    > This directory contains functions called in subsequent steps  
<br>  
2. Add [/SVM_scripts/scripts_from_zaixu](https://github.com/sheilashanmugan/funcParcelSexDiff/tree/gh-pages/SVM_scripts/scripts_from_zaixu) to matlab path.  

    > This directory contains functions called in subsequent steps  
<br>
3. Run [/SVM_scripts/makeNonZeroMatrix.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/makeNonZeroMatrix.m) to make nonzero index.   

    > This script creates the non-zero index needed to visualize results  
<br>
4. Run [/SVM_scripts/run_SVM/SVM_sex_2fold_CSelect_Cov_SubIndex_Perm_20200719.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/run_SVM/SVM_sex_2fold_CSelect_Cov_SubIndex_Perm_20200719.m) with matlab open to run SVM  

    > This frist part of this script (100 Repeat) submits 100 jobs, each of which are one of the 100 repetitions of SVM predictions. when you run , the 100 jobs will be submitted. The second part of this script (Permutation, 1000 times) submits 1000 jobs, each of which are one of 1000 permutations that will be used for significane testing of accuracy.  
<br>
5. Run [/SVM_scripts/calc_accuracy/average_acc_sens_spec_SVM_multiTimes_20200720.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/calc_accuracy/average_acc_sens_spec_SVM_multiTimes_20200720.m) to calculate summary measures and assess significance 

    > This script calculates the average accuracy, sensitivity, specificity from the 100x repeats of SVM. It also calculates a p value for accuracy.  
<br>
6. Run [/SVM_scripts/Weight_Visualize_Workbench/Weight_Visualize_Workbench_Sex_SVM2foldCSelectCovSubIndex_20200721.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/Weight_Visualize_Workbench/Weight_Visualize_Workbench_Sex_SVM2foldCSelectCovSubIndex_20200721.m) to create files for workbench visualization  

    > This script creates files for workbench visualization. It creates ciftis that display the weight of the first 25% regions with the highest absolute weight from SVM (Figure 3C). It also creates a cift that displays the sum of the absolute value of weight across the 17 networks (Figure 3D).  
<br>

7. Visualize files in workbench

    > /Applications/workbench/bin_macosx64/wb_view /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/spec_files/rh.inflated.surf.gii /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/spec_files/lh.inflated.surf.gii /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/Group_Loading_17Networks/*dscalar* /cbica/projects/funcParcelSexDiff/results/PredictionAnalysis/SVM/2fold_CSelect_Cov_SubIndex/Sex_CovAgeMotion/Permutation/res_MultiTimes/AtlasLoading/WeightVisualize_Sex_SVM_2fold_CSelect_Cov_MultiTimes/First25Percent/w_Brain_Sex_First25Percent_Network_*.dscalar.nii /cbica/projects/funcParcelSexDiff/results/PredictionAnalysis/SVM/2fold_CSelect_Cov_SubIndex/Sex_CovAgeMotion/Permutation/res_MultiTimes/AtlasLoading/WeightVisualize_Sex_SVM_2fold_CSelect_Cov_MultiTimes/w_Brain_Sex_Abs_sum.dscalar.nii &
<br>

### Univariate approach
1. Submit [/atlasLoadingScripts/sexEffect_atlasLoading_20200612.R] (https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/atlasLoadingScripts/sexEffect_atlasLoading_20200612.R) to qsub to calculate effect of sex on atlas loadings  

    > This script aggregates atlas loadings for all subjects, runs a GAM at each vertex to determine the effect of sex while controlling for age and motion, then corrects for multiple comparisons. 

