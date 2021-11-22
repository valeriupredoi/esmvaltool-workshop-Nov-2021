## Automatic download of CMIP/obs4MIPS/CORDEX data

- Running ESMValTool on an HPC close to an ESGF data node, or home on yer mum's laptop;
- What to do when the user needs data that is not present on the ESGF node or your mum has no data? 
- Average CMIP data coverage on ESGF: about 90% 
- The missing odd 10% may amount to hundreds of Gigabytes of missing data
- One of the main purposes of ESMValTool is to provide fully reproducible results, no matter the site where it is run at - you need all the data from given recipe
- -> Automatic data download functionality in ESMValTool:
  - uses [ESGF Pyclient](https://esgf-pyclient.readthedocs.io/en/latest/);
  - the ESGF Pyclent tool is installed as a dependency of ESMValCore from PyPi
  - and provides an interface and API allowing the download of data from various remote ESGF nodes
  - provides API for interactions with the ESGF Search [REST API](https://github.com/ESGF/esgf.github.io/wiki/ESGF_Search_REST_API)
- Configuration of this backend from the user perspective is [minimal](https://docs.esmvaltool.org/projects/esmvalcore/en/latest/quickstart/configure.html#config-esgf):
  - minimally, use either a flag to the run command `--offline=False` or set `offline: false` in `config-user.yml`, and a download directory e.g. `download_dir: ~/climate_data` in `config-user.yml`; `offline` runtime flag is off/True by default;
  - set `rootpath` and `drs` parameters in `config-user.yml` to use downloaded data:
```yaml
rootpath:
  CMIP6: ~/climate_data
  CMIP5: ~/climate_data
  default: ~/climate_data
drs:
  CMIP6: ESGF
  CMIP5: ESGF
```
- Performance refinements:
  - Downloading data from the *closest* ESGF node, based on measuring the shortest server response time
  - Automatic detection of data that had already been downloaded, so that next time ESMValTool runs, that data will just be read off disk

## Reusing preprocessed data: first steps towards distributed ESMValTool

To run aa recipe in a semi-distributed way, the commands shown below were used.

- ran on Mistral at DKRZ:
```
esmvaltool run recipe_distributed.yml --diagnostics map/tas_dkrz --remove_preproc_dir=False --run_diagnostic=False
```
  this resulted in a number of log messages, with the following message indicating the output directory for the preprocessed data:
```
2021-11-18 16:22:41,906 UTC [2277] INFO        PREPROCDIR = /pf/b/b381141/esmvaltool_output/recipe_distributed_20211118_162237/preproc
```
- ran on Jasmin at CEDA:
```
esmvaltool run recipe_distributed.yml --diagnostics map/tas_ceda --remove_preproc_dir=False --run_diagnostic=False
```
  this resulted in a number of log messages, with the following message indicating the output directory for the preprocessed data:
```
2021-11-18 16:18:27,104 UTC [27364] INFO    PREPROCDIR = /work/scratch-pw/bandela/esmvaltool_output/recipe_distributed_20211118_161821/preproc
```
- finally, jointhese two runs together into one single, local, diagnostic run:
```
esmvaltool run recipe_distributed.yml --resume_from "~/distributed/recipe_distributed_20211118_161821 ~/distributed/recipe_distributed_20211118_162237"
```
