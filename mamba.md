## Installation with Mamba 🐍

- See [ESMValTool documentation](https://docs.esmvaltool.org/en/latest/quickstart/installation.html#install-with-pip) (by yours faithfuls Klaus and V) how to do the Installation dance with mamba;
- Mamba 🐍 is a fast (C++ backend) alternative to `conda` (SAT solver much, much faster);
- Tested thoroughly in [ESMValTool](https://github.com/ESMValGroup/ESMValCore/issues/1227);
- API/CLI identical to `conda`'s API/CLI: e.g. to create a new environment from an environment file `environment.yml` (`conda` vs `mamba`):

```bash
conda env create -n esmvaltool -f environment.yml
mamba env create -n esmvaltool -f environment.yml
```
- Use Miniforge (see ESMValTool installation documentation) or usual Anaconda/Miniconda
  then install `mamba` in the `(base)` environment with `conda install -c conda-forge mamba`;
  use `mamba` instead of `conda` as CL executable **apart from activating an environment**,
  there you should still use `conda activate esmvaltool`;
- Try to use latest version (currently 0.18.1, see [package on conda-forge](https://anaconda.org/conda-forge/mamba));
- Deployed in Github Action [tests](https://github.com/ESMValGroup/ESMValTool/runs/4282316666?check_suite_focus=true) and Circle CI [tests](https://app.circleci.com/pipelines/github/ESMValGroup/ESMValTool/5991/workflows/b28d8bee-991f-4c9c-bd34-9679664157c4/jobs/51652);
