# Polarization-conditioned Fourier-enhanced DeepONet (PC-FDON)
Model and code of Polarization-conditioned Fourier-enhanced DeepONet (PC-FDON) for Electric field reconstruction from EFISH measurements in both polarization (vertical and horizontal) directions.


## Model file (PC-FDON) and model description
To be available soon

## Instructions to use the model for Efield prediction:
1. To use the model, please first interpolate the EFISH file to the following grid via MATLAB:
   $z/z_R = [-50:2:-24 \, -22:1:-16 \, -15:0.5:-1.5 \, -1:0.2:1 \, 1.5:0.5:15 \, 16:1:22 \, 24:2:50]$; <br>

2. Then further normalize the $z/z_R$ by dividing $z_\mathrm{scale} = 50$, then the input grid should be:
   $z^\prime = z/z_R/50 = [-50:2:-24 \, -22:1:-16 \, -15:0.5:-1.5 \, -1:0.2:1 \, 1.5:0.5:15 \, 16:1:22 24:2:50]/50$; <br>

**Note 1**: $z^\prime \in [-1,1]$; crop the input EFISH profile if the normalized and scaled range (z/z_R/50) goes beyond this range. <br>

**Note 2**: The sampling grid outside your experiment range could be set to zero, as the DDON accepts zero input outside the key feature range. For how to quantify the key range, please refer to our paper. Please ensure the input range is no less than 4.2*FWHM of your input EFISH profile (normalized), although sometimes a smaller sampling range than the criterion also works. <br>

3. Normalize the measured EFISH profile (along the laser propagation axis, $z$) by its maximum:
   $P_\mathrm{norm}(z) = P(z)/P_\mathrm{max}$ <br>

4. Estimate the phase mismatch value $u$ through the wave-factor mismatch $\Delta k$ and Rayleigh range $z_\mathrm{R}$, and normalize it as input:
   $u^\prime$ = $\Delta k \cdot z_\mathrm{R}$/-1. <br>
   **Note**: -1 is the min $u$ value from the training dataset; if you retrain the model, please use the min $u$ from your own dataset. <br>

5. Define the polarization label $\Psi$ as the input. A preset value of 0 denotes vertical polarization, and a value of 1 denotes horizontal polarization.

6. Import the MAT file as structure files and obtain the prediction. Or you can modify the code to fit your data structure as well.

   The MAT file structure is as follows:<br>
   
   <table>
  <tr>
    <td rowspan="3"><code>Profile_Px</code></td>
    <td>$P_x$</td>
    <td>$[z, P_x]$ (experimentally measured EFISH and normalized $z'$; dim: [109,2])</td>
  </tr>
  <tr>
    <td>$u$</td>
    <td>The phase mismatch value along $z$; dim: [109,1]</td>
  </tr>
  <tr>
    <td>$\text{label_cls}$</td>
    <td>Polarization labels: 0-vertical, 1-horizontal; dim: [109,1]</td>
  </tr>
  <tr>
    <td>$E_x$</td>
    <td>The electric field value along $z$; dim: [109,1]</td>
  </tr>
</table>

