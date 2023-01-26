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
