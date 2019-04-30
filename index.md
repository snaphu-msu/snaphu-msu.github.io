## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/snaphu-msu/snaphu-msu.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

## Getting started with supernova simulations in FLASH

We're starting this tutorial with the assumption that you have successfully downloaded the FLASH code and installed all of the necessary compilers and libraries.  For more information on system requirements, see the [FLASH User Documentation](http://flash.uchicago.edu/site/flashcode/user_support/flash4_ug_4p6/).  We recommend running the Sedov test problem laid out in [Section 2.4](http://flash.uchicago.edu/site/flashcode/user_support/flash4_ug_4p6/node15.html) of the User Documentation to ensure that everything is properly installed.

### Setting up your simulation

The following setup line will give you a basic CCSN simulation:

    ./setup CoreCollapse/M1 -auto -1d +spherical -nxb=12 -objdir obj_ccsn1dSpark threadBlockList=False threadWithinBlock=False +newMpole +spark m1_groups=12 --with-unit=physics/RadTrans/RadTransMain/M1 -maxblocks=500

There are many variations on the above setup parameters, based on the dimensionality and other specifics of the simulations that you want to run.  This will set up a 1D, spherically symmetric simulations using M1 neutrino transport, the multipole gravity solver, and the Spark hydro solver.

Once the setup is complete and the code is compiled, there are a few more things you will need before you can run a CCSN simulation:

#### Progenitor model

This file provides the "initial conditions" for the simulation. There are several progenitor models available in the `source/Simulation/SimulationMain/CoreCollapse/progenitors` directory.  There will be a header that specifies which variables are included in the initial conditions, followed by a table with the values of those variables through the progenitor.

#### Equation of state table

Equation of state tables are available in FLASH compatible format from [www.stellarcollapse.org](www.stellarcollapse.org).  

#### Neutrino opacity table

#### Parameter file

A sample parameter file can be found in `source/Simulation/SimulationMain/CoreCollapse/M1` called `M1_1D_input.par`, which will provide a good basic simulation setup.  The parameter file is broken down by different parts of the code (or physics), e.g. hydro, radiation transport, gravity, etc.  Most of the parameters in the sample parameter file should be fine for a basic simulation.

The name of the progenitor model that you chose above should be specified by the `model_file` parameter.  The equation of state table name and neutrino opacity table should be specified by `eos_table` and `rt_opacTable` parameters respectively.

### Running the simulation

In order to run the simulation, all of the above, along with the compiled exectuble, should be in the same directory.  You will need a subdirectory named as specified by the `output_directory` parameter in the parameter file.

Now it's time to run the code!  There will be two types of output.  They will all be named after the `basenm` parameter in the `flash.par` file.  The _basenm_.dat file, which will be in the same directory as the code is run in, will provide integrated quantities over the star at timestep intervals specified by the `wr_integrals_freq` parameter.  This is where you will find things such as the shock radius and explosion energy.  Everything stored in this file is specified in the header.  Note that the time indicated in this file is the _simulation_ time and not necessarily time _post-bounce_.

There will also be HDF5 plot and checkpoint files in the `output_directory`.  The checkpoint files (`_hdf5_chk_cnt_`) store all of the variables necessary to restart a simulation.  The plot files (`_hdf5_plt_cnt_`) contain _only_ the variables specified by `plot_var` in the parameter file.

To restart a simulation from a checkpoint file, in the parameter file, change `restart` to True and `checkpointFileNumber` to the number of the checkpoint file that you want to restart from.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
