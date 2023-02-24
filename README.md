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

`theory_camb_high_accuracy.yaml` uses the high accuracy settings for CAMB noted in [Hill et al. (2020)](https://arxiv.org/abs/2109.04451).

`theory_cosmopower.yaml` uses [Cosmopower](https://arxiv.org/abs/2106.03846) as implemented in the _Simons Observatory_ package [SOLikeT](https://github.com/simonsobs/SOLikeT).

`params_foreground_Choi2020.yaml` uses the foreground model implemented in MFLike based on the [Choi et al. (2020)](https://arxiv.org/abs/2007.07289) foreground model.

# Codes used

## Likelihood

The current files are set to be used with the **MFLike** likelihood from the Simons Observatory (SO). Right now, it is set up to be coupled to the main branch release of [the public git](https://github.com/simonsobs/LAT_MFLike).

In the future we intend to release a public **ACT DR6** likelihood implementation based on MFLike. Once we are finalizing this code, we will converge these parameters and settings files to this likelihood.

## Theory

For cosmological observables, there are two options to be used right now. One can either use [camb](https://github.com/cmbant/CAMB) as it exists within cobaya, or you can use the [cosmopower](https://arxiv.org/abs/2106.03846) code that exists in [SOLikeT](https://github.com/simonsobs/SOLikeT), the likelihood and theory code library developed by the SO collaboration.

### CosmoPower: 'KeyError: _manual' bug

(TL;DR: see [this](https://github.com/CobayaSampler/cobaya/pull/275) pull request if you get this error).

If you are using **Cobaya version 3.2.1** along with `SOLikeT.CosmoPowerDerived`, then you will likely run into a cryptic error message that ends with `KeyError: _manual`. This bug is caused by some internal cobaya logic. A fix for this bug can be found [here](https://github.com/CobayaSampler/cobaya/pull/275). It is currently accepted into the main development branch for Cobaya, so any future releases should fix this error.
