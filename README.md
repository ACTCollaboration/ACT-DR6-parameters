# ACT DR6 parameters
Parameters and run settings for ACT DR6 using cobaya.

## File Structure

All yaml files are top-level. To perform a run with cobaya, you can invoke

```
[python -m] cobaya run run_<file>.yaml
```

Where `run_<file>.yaml` is any one of the run files contained here.

If you want to add new files, please consider the following style guides:

- Run files start with `run_`.

- Theory code files start with `theory_`.

- Likelihood files start with `likelihood_`.

- Parameter files start with `params_`. You can include priors on your parameters, but please only use physical bounds. External priors (i.e. priors from external analyses) are to be included in prior files. It is recommended to put references on parameters.

- Prior files start with `priors_`. These contain external priors only.

## Notes on existing files

`theory_camb_high_accuracy.yaml` uses the high accuracy settings for CAMB at least as accurate as in [Hill et al. (2020)](https://arxiv.org/abs/2109.04451). Note that we intend to use `cosmorec` instead of `recfast` as main recombination code - see the section _Installing Cosmorec_ below for instructions.

`theory_cosmopower.yaml` uses [Cosmopower](https://arxiv.org/abs/2106.03846) as implemented in the _Simons Observatory_ package [SOLikeT](https://github.com/simonsobs/SOLikeT). See the section _Setting up CosmoPower_ on instructions of setting this up.

`params_foreground_Choi2020.yaml` uses the foreground model implemented in MFLike based on the [Choi et al. (2020)](https://arxiv.org/abs/2007.07289) foreground model.

`params_foreground_sims.yaml` uses the same foreground model as above, but allows parameters to go outside physical constraints (i.e. negative for values that are expected to be non-negative).

`params_DR4_baseline.yaml` is a simple set of fixed parameters that can be used to evaluate your model at the *ACT DR4+WMAP baseline* parameters found in Choi et al. and Aiola et al.

# Codes used

## Likelihood

The current files are set to be used with the **MFLike** likelihood from the Simons Observatory (SO). Right now, it is set up to be coupled to the main branch release of [the public git](https://github.com/simonsobs/LAT_MFLike).

We are using the [ACT DR6 MFLike](https://github.com/ACTCollaboration/act_dr6_mflike) likelihood that functions as a DR6-specific implementation for MFLike.

## Theory

For cosmological observables, there are two options to be used right now. One can either use [camb](https://github.com/cmbant/CAMB) as it exists within cobaya, or you can use the [cosmopower](https://arxiv.org/abs/2106.03846) code that exists in [SOLikeT](https://github.com/simonsobs/SOLikeT), the likelihood and theory code library developed by the SO collaboration.

### Installing CosmoRec

We use the recombination code [CosmoRec](http://www.jb.man.ac.uk/~jchluba/Science/CosmoRec/CosmoRec.html) for camb. By default, camb only comes with the recombination code Recfast installed, but this code is not accurate enough for our purposes. To install CosmoRec, follow these instructions:

1. Install camb (v1.5.0 or higher) for cobaya.

2. Download and untar CosmoRec via:

```
wget https://www.cita.utoronto.ca/~jchluba/Recombination/_Downloads_/CosmoRec.v2.0.3b.tar.gz
tar -xvf CosmoRec.v2.0.3b.tar.gz
```

3. In the location where you installed camb (this will either be a directory where you have a separate instance of camb, or a directory within your cobaya packages path), locate the `fortran/Makefile_main` file. Add the following two lines all the way at the top:

```
RECOMBINATION_FILES = recfast cosmorec
COSMOREC_PATH = <path/to/cosmorec>
```

Where `<path/to/cosmorec>` is the path where you unpacked CosmoRec in step 2.

4. Recompile the fortran library by invoking

```
python setup.py make
```

in the main `camb` directory. This should compile camb with CosmoRec enabled.

### Setting up CosmoPower

If you intend to use the CosmoPower theory code, you should modify the `theory_cosmopower.yaml` file to make sure that the theory code can find your networks. The file is setup for use of the emulators described in [Bolliet et al. (2023)](https://arxiv.org/abs/2303.01591), and you only need to change the two lines that start with `network_path` to the path where you stored the emulators on your machine.

If you intend to use this code with your own emulators, check out the [SOLikeT documentation](https://soliket.readthedocs.io/en/latest/cosmopower.html) on the CosmoPower wrapper for instructions.

### CosmoPower: 'KeyError: _manual' bug

**This bug has been fixed as of cobaya verison 3.3.**

(TL;DR: see [this](https://github.com/CobayaSampler/cobaya/pull/275) pull request if you get this error).

If you are using **Cobaya version 3.2.1** along with `SOLikeT.CosmoPowerDerived`, then you will likely run into a cryptic error message that ends with `KeyError: _manual`. This bug is caused by some internal cobaya logic. A fix for this bug can be found [here](https://github.com/CobayaSampler/cobaya/pull/275). It is currently accepted into the main development branch for Cobaya, so any future releases should fix this error.
