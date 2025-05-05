# RVQC
A repository for the code used in the paper "A Universal Quantum Computer From Relativistic Motion."

To reproduce the figure in the quantum Fourier transform section of the supplementary material from the data contained in the Training Data folder, run the following python script:
```
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
from mpl_toolkits.axes_grid1.inset_locator import inset_axes, mark_inset
import seaborn as sns

data = np.load('Loss_Fidelity_Seed.npz')

new_fid = data['fvals']
new_loss = data['lvals']

# Used for representation of fluctuations in learning curves
fid_means = np.mean(new_fid, axis=0)
fid_stds = np.std(new_fid, axis=0)
loss_means = np.mean(new_loss, axis=0)
loss_stds = np.std(new_loss, axis=0)

# Create the average fidelity and loss plots
fig, ax = plt.subplots(2, 1, figsize=(11.0, 15.0), dpi=200)
ax[0].set_ylabel('Average Fidelity')
ax[0].set_xlabel('Step Number')
ax[1].set_ylabel('Average Loss')
ax[1].set_xlabel('Step Number')
colors = sns.color_palette("husl", 2)
plot_cut_fid = len(fid_means)-2
plot_cut1_fid = 0
plot_cut_loss = len(loss_means)-2
plot_cut1_loss = 0

with sns.axes_style("darkgrid"):
    fid_means_cut = fid_means[plot_cut1_fid:plot_cut_fid]
    fid_stds_cut = fid_stds[plot_cut1_fid:plot_cut_fid]
    x_fid = np.arange(0, 30000, step=100)
    loss_means_cut = loss_means[plot_cut1_loss:plot_cut_loss]
    loss_stds_cut = loss_stds[plot_cut1_loss:plot_cut_loss]
    x_loss = np.arange(len(loss_means_cut))

    # Define the region to magnify
    xf1, xf2, yf1, yf2 = 0, 30000, 0.94, 1.0
    xl1, xl2, yl1, yl2 = -100, 30000, -0.0001, 0.1
    
    # Create inset for the magnified region
    ax_inset = inset_axes(ax[0], width="65%", height="65%", loc="lower right", borderpad=5)
    ax_inset.plot(x_fid, fid_means_cut, c=colors[0])
    ax_inset.fill_between(x_fid, y1=fid_means_cut-fid_stds_cut, y2=fid_means_cut+fid_stds_cut ,alpha=0.3, facecolor=colors[0])
    ax_inset.set_xlim(xf1, xf2)
    ax_inset.set_ylim(yf1, yf2)
    ax_inset.grid(False)
    ax_inset1 = inset_axes(ax[1], width="65%", height="65%", loc="upper right", borderpad=3)
    ax_inset1.plot(x_loss, loss_means_cut, c=colors[1]) 
    ax_inset1.fill_between(x_loss, y1=loss_means_cut-loss_stds_cut, y2=loss_means_cut+loss_stds_cut ,alpha=0.3, facecolor=colors[1])
    ax_inset1.set_xlim(xl1, xl2)
    ax_inset1.set_ylim(yl1, yl2)
    ax_inset1.grid(False)
    
    # Mark the inset regions on the main plots
    mark_inset(ax[0], ax_inset, loc1=1, loc2=4, fc="none", ec="0.8")
    mark_inset(ax[1], ax_inset1, loc1=1, loc2=4, fc="none", ec="0.8")
    ax[0].plot(x_fid, fid_means_cut, c=colors[0]) 
    ax[0].fill_between(x_fid, y1=fid_means_cut-fid_stds_cut, y2=fid_means_cut+fid_stds_cut ,alpha=0.3, facecolor=colors[0])
    ax[1].plot(x_loss, loss_means_cut, c=colors[1]) 
    ax[1].fill_between(x_loss, y1=loss_means_cut-loss_stds_cut, y2=loss_means_cut+loss_stds_cut ,alpha=0.3, facecolor=colors[1])
    ax[1].set_ylim(-0.1, max(loss_means_cut)+0.2)
    
plt.show()
```
