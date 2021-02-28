<br>
<br>
# Sex Differences in Functional Topography of Association Networks
*Prior work has shown that there is substantial interindividual variation in the spatial distribution of functional networks across the cerebral cortex, or “functional topography”. However, it remains unknown whether there are normative developmental sex differences in the topography of individualized networks. Here we leverage regularized non-negative matrix factorization to define individualized functional networks in 693 youth ages 8-23y imaged with fMRI as part of the Philadelphia Neurodevelopmental Cohort. Support vector machines were applied to examine sex differences in multivariate patterns of functional topography. Generalized additive models with penalized splines were then used to examine the impact of sex on topography at a more granular level. We used chromosomal enrichment analyses to assess the correlation between gene expression (Allen Human Brain Atlas) and sex differences in functional topography, evaluating significance with gene-wise non-parametric permutation tests. This project identifies normative developmental sex differences in the functional topography of association networks and highlight the role of sex as a biological variable in shaping functional brain development in youth.*

### Project Lead
Sheila Shanmugan

### Faculty Lead
Theodore D. Satterthwaite  

### Analytic Replicator
Zaixu Cui (imaging)    
Jakob Seidlitz (genetics)

### Collaborators 
Jakob Seidlitz, Zaixu Cui, Azeez Adebimpe, Danielle S. Bassett, Maxwell A. Bertolero, Christos Davatzikos, Damien A. Fair, Raquel E. Gur, Ruben C. Gur, Hongming Li, Adam Pines, Armin Raznahan, David R. Roalf, Russell T. Shinohara, Jacob Vogel, Daniel H. Wolf, Yong Fan, Aaron Alexander-Bloch  

### Project Start Date
January 2019

### Current Project Status
In preparation

### Datasets
PNC

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

    > This frist part of this script (100 Repeat) submits 100 jobs, each of which are one of the 100 repetitions of SVM predictions. The second part of this script (Permutation, 1000 times) submits 1000 jobs, each of which are one of 1000 permutations that will be used for significane testing of accuracy.  
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

8. Calculate summary statistics with [SVM_scripts/calc_accuracy/average_acc_sens_spec_SVM_multiTimes_20200720.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/calc_accuracy/average_acc_sens_spec_SVM_multiTimes_20200720.m)

    > This script aggregates accuracy, sensitivity, and specificity across the 100 SVM repeats then averages them. It then compares this accuracy to permuted accuracies to calculate a p-value for accuracy. Save `acc_AllModels1_perm` as `SVM_perm_accuracy.csv` to generate histogram inset in step 11 below.
<br>

9. Get y values needed to draw ROC with [SVM_scripts/roc/yvalues_100_20201108.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/roc/yvalues_100_20201108.R)

    > This script aggregates the Y values across the 100 SVM repeats. The output of this step is used in step 10 below.
<br>

10. Create ROC with [SVM_scripts/roc/SVM_ROC.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/roc/SVM_ROC.m)

    > This script uses the function [/SVM_scripts/roc/AUC_Calculate_ROC_Draw2.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/roc/AUC_Calculate_ROC_Draw2.m) to create the plot in Figure 3a.
<br>

11. Create histogram inset in figure 3a with [SVM_scripts/roc/SVM_accuracy_histogram.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/roc/SVM_accuracy_histogram.R)

    > This script creates a histogram of accuracies from the permutation test.
<br>

12. Create barplot of SVM Weights with [SVM_scripts/barplots/sums_weights_stackedBarplot_20201021.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/barplots/sums_weights_stackedBarplot_20201021.R)

    > This script creates the plot in Figure 3b
<br>
  
### Univariate approach
1. Submit [atlasLoadingScripts/sexEffect_atlasLoading_20200612.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/atlasLoadingScripts/sexEffect_atlasLoading_20200612.R) to qsub to calculate effect of sex on atlas loadings   

    > This script aggregates atlas loadings for all subjects, runs a GAM at each vertex to determine the effect of sex while controlling for age and motion, then corrects for multiple comparisons.  
    > qsub -l h_vmem=10.5G,s_vmem=10.0G /cbica/projects/funcParcelSexDiff/scripts/run_R_script.sh /cbica/projects/funcParcelSexDiff/scripts/atlasLoadingScripts/sexEffect_atlasLoading_20200612.R
<br>

2. Write effect map for each network with [WriteEffectMap/WriteEffectMap_Workbench_Sex_20200712.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/WriteEffectMap/WriteEffectMap_Workbench_Sex_20200712.m)

    > This script takes the output of the GAMs from step 1 and creates the CIFTI files of the effect of sex at each vertex. 
<br>

3. Create Figure 4A with [WriteEffectMap/WriteEffectMap_Workbench_Sex_AbsSum_20200712.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/WriteEffectMap/WriteEffectMap_Workbench_Sex_AbsSum_20200712.m)

    > This script sums the absolute value of the effect of sex across all 17 networks and creates a CIFTI file of this effect.
<br>

4. Visualize files in workbench (Figure 4A, 4D, and 4E)

    > /Applications/workbench/bin_macosx64/wb_view /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/spec_files/rh.inflated.surf.gii /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/spec_files/lh.inflated.surf.gii /Users/sheilash/Desktop/projects/pfn_sex_diff/inputData/Group_Loading_17Networks/*dscalar* /cbica/projects/funcParcelSexDiff/results/GamAnalysis/AtlasLoading/SexEffects/Gam_Z_FDR_Sig_Vector_All_Sex_Network_* /cbica/projects/funcParcelSexDiff/results/GamAnalysis/AtlasLoading/SexEffects/UnthreshAbsSum/Gam_Sex_Abs_sum.dscalar.nii &
<br>

5. Aggregate group level matricies with [WriteEffectMap/barplots/Make_SexEffects_mat_gam_FDR_sig.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/WriteEffectMap/barplots/Make_SexEffects_mat_gam_FDR_sig.m)

    > This script aggregates group level matricies for each network that denote significant verticies. The output of this step is the input for step 6.
<br>

6. Create barplot in Figure 4C with [/SVM_scripts/barplots/sums_weights_stackedBarplot_20201021.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/SVM_scripts/barplots/sums_weights_stackedBarplot_20201021.R)

    > This script creates a barplot of the number of verticies that survives FDR correction for each network.
<br>

### Spin test to compare results from multivariate pattern analysis (Figure 3D) and GAMs (Figure 4A)
1. Download and add [spintest/scripts/](https://github.com/sheilashanmugan/funcParcelSexDiff/tree/gh-pages/spintest/scripts) to matlab path.  

    > This directory contains functions called in subsequent steps. These functions were originally downloaded from [here](https://github.com/spin-test/spin-test)  
<br>
    
2. Prepare data for spin test with [spintest/prepare_data_for_spintest_20201104.R](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/spintest/prepare_data_for_spintest_20201104.R)  

    > This script formats data for the spin test.
<br>

    
3. Run spin test with [spintest/spinSVMvsGAM.m](https://github.com/sheilashanmugan/funcParcelSexDiff/blob/gh-pages/spintest/spinSVMvsGAM.m)  

    >This script runs the spin test to compare the map of summed absolute prediction weights from our machine learning model (Figure 3D) to a map of GAM effect size (Figure 4A) 
<br>
