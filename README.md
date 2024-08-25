---
title: GSOC final submission report

---

![Google summer of code](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjP9mSaWLVqq3LSI-TQB6u-YcHCga03BTo2SJ12SGHBbykk6BTT-Vu3kWn5gKWdRQcd-Em_D3CSFh387lN_cHqjktA84KG3YXgIOevo4NfV5lV23_BOLP_ndIiA_f-Yy4BKPTF3UZRsxcyp-1fv_cNo9B60Xkk-i1pOYjhGOCZkspcIvD36J61mDmOttlA/s1600/GSoC%20Banner.png)

![sktime](https://www.sktime.net/en/v0.15.1/_static/sktime-logo-text-horizontal.png)


### Goals:

1. Extending support to polars Data container in sktime with sktime supported scitypes i.e Table, Series, Panel and Hierarchical. 
2. Implement Foundation models in sktime.
3. Enhance HFtransformers by interfacing PEFT methods.

### Mentors : [Benedikt Heidrich](https://github.com/benHeid), [Franz Király](https://github.com/fkiraly) 


## My contributions

### Polars support

The work done here enables users of sktime to pass polars dataframe in there desired estimator and also get output in polars format. This works by converting the polars frame to the inner mtype supported by sktime estimator and performing fit and predict on that. 


| Description | PR | Status |
| -------- | -------- | -------- |
| Polars conversion utilities| [sktime/#6455](https://github.com/sktime/sktime/pull/6455)    | Merged, Approved     |
| Series scitype support for polars | [sktime/#6485](https://github.com/sktime/sktime/pull/6485) | Merged, Approved
| Panel scitype support for polars | [sktime/#6552](https://github.com/sktime/sktime/pull/6552) | Under Review |
| Heirarchical scitype support for polars| [sktime/#6697](https://github.com/sktime/sktime/pull/6697) | Under Review


**Notebook**
[Notebook demonstrating polars mtypes examples and conversions](https://github.com/pranavvp16/GSOC-submission/blob/main/notebooks/polars_support.ipynb)

**Challenges**

Representing index columns in polars dataframe was a main challenge as polars didn't support index columns. Therefore while making conversions from other datatypes to polars and vice-versa it became necessary to retain the index columns by representing the index columns with notation `__index__{index_name}` in the polars frame.


### Moirai forecaster

Added salesforce moirai implementation in sktime. This implementation uses the moirai weights released under permissive license, hosted by sktime.



| Description | PR | Status |
| -------- | -------- | -------- |
| Hosting moirai small variation weights on hugging face     |  [sktime/moirai-1.0-R-small](https://huggingface.co/sktime/moirai-1.0-R-small)     | Merged, Approved     |
|Hosting moirai base variation weights on hugging face | [sktime/moirai-1.0-R-base](https://huggingface.co/sktime/moirai-1.0-R-base) | Merged, Approved |
|Hosting moirai large variation weights on hugging face | [sktime/moirai-1.0-R-large](https://huggingface.co/sktime/moirai-1.0-R-large) | Merged, Approved
| Add Moirai Forecaster to sktime | [sktime/#6746](https://github.com/sktime/sktime/pull/6746) | Under Review

The PR adding `moirai_forecaster` is currently under review, due to delay in work because of incompetencies in weights and MOIRAI package i.e uni2ts not hosted on PyPi. 

**Notebook**

[Notebook demonstrating interface of MOIRAI](https://github.com/pranavvp16/GSOC-submission/blob/main/notebooks/forecasting_with_moirai.ipynb)

**Challenges**

1. While work for MOIRAI implementation was under progress Salesforce updated the weights of moirai to `cc-by-nc-4.0` license changing the format from `.ckpt` to `.safetensors`. This lead to a [discussion](https://github.com/sktime/sktime/issues/6681) about whether sktime should support the latest weights as the were non-permissible for comercial use. After some community feedback and discussion with core-developers of sktime, we decided to host the snapshot of old apache weights on sktime's hugging face and continue supporting the old weights.
2. The package in which MOIRAI lives is [uni2ts](https://github.com/SalesforceAIResearch/uni2ts) from SalesforceAIReasearch. Due to uni2ts not being hosted on PyPi I had to vendor the code specific to MOIRAI in `sktime/libs` and make it compatible with sktime's MOIRAI implementation.
3. The old weights of MOIRAI where serialized along with imports to original classes in `uni2ts` which I had to patch and redirect the imports to `sktime/libs/uni2ts`.

### Some minor enhancments, bugs and fixes


| Description | PR/Issue | Status |
| -------- | -------- | -------- |
| NeuralForecast LSTM model     | [sktime/#6047](#https://github.com/sktime/sktime/pull/6047)    | Marged, Approved  |
| Interface AutoLSTM from neuralforecast| [sktime/#6107](https://github.com/sktime/sktime/pull/6107) | PR in progress|
| Update metadict for polars mtype| [sktime/#5423](https://github.com/sktime/sktime/issues/5423) | Closed
| Fix nested_univ handling index names | [sktime/#7026](https://github.com/sktime/sktime/pull/7026) | Under Review


### Deviations in proposed work

Intially I proposed to work on interfacing pytorch-forecasting in sktime, but this work was completed by Xinyu Wu(other GSOC contributor) in [PR #6228](https://github.com/sktime/sktime/pull/6228) due to deduplicating proposals, instead of which I worked on adding the moirai forecaster. Similarly the work for enhancing the hugging-face adapter to support PEFT was done by Armaghan in [PR #6457](https://github.com/sktime/sktime/pull/6457)

### Acknowlegdements

Google Summer of Code has played a phenomenal role in my open source journey. I thank sktime and GSOC for granting me this opportunity. I am grateful to my mentors for their continuous guidance and encouragement. Without their assistance this project would not have been possible. Throughout the summer I contributed in sktime across, datatypes and forecasting directory of sktime with contributions ranging from polars data-container for sktime and transformer based forecasters.