![detritalPy-logo](https://github.com/grsharman/detritalPy/blob/master/detritalPy_logo.svg)

## Description

detritalPy is a Python module for visualizing and analyzing detrital geo-thermochronologic data. Designed to be implemented via a Jupyter Notebook, detritalPy aims to provide an efficient means of processing and analyzing large detrital mineral isotopic and geochemical datasets. For more information, please refer to [Sharman et al., 2018](https://doi.org/10.1002/dep2.45).

## Installation

<code>pip install detritalpy</code>

## Upgrading

<code>pip install detritalpy --upgrade</code>

## Requirements

Installation of the open data science platform Anaconda by Continuum Analytics will provide most of the required Python modules needed to run detritalPy. The following is a full list of dependencies for all detritalPy functions: 

* numpy
* matplotlib
* pandas
* xlrd
* folium
* vincent
* simplekml
* scipy
* sklearn
* statsmodels
* peakutils

## Data Formatting

detritalPy requires that input data be organized using a specific format. Example datasets can be found in the [example-data folder](https://github.com/grsharman/detritalPy/blob/master/example-data.zip), and additional information is provided in the [detritalPy manual](https://github.com/grsharman/detritalPy/blob/master/detritalPy_manual_0.0.1.zip).

## Data Import and Selection

One or more spreadsheets can be simultaneously imported using a number of different ways of specifying the file path.

```python
# Import relative file pathway(s)
from pathlib import Path

# Specify file paths to data input file(s)
dataToLoad = [Path("example-data/") / "ExampleDataset_1.xlsx",
              Path("example-data/") / "ExampleDataset_2.xlsx"]

main_df, main_byid_df, samples_df, analyses_df = dFunc.loadDataExcel(dataToLoad)
```

Or filepaths can be written this way if using a PC:

```python
# Specify file paths to data input file(s)
dataToLoad = [r'C:\Users\gsharman\Documents\GitHub\detritalPy\example-data\ExampleDataset_1.xlsx',
              r'C:\Users\gsharman\Documents\GitHub\detritalPy\example-data\ExampleDataset_2.xlsx']

main_df, main_byid_df, samples_df, analyses_df = dFunc.loadDataExcel(dataToLoad)
```

Or this way if using a Mac:

```python
# Specify file paths to data input file(s)
dataToLoad = ['/Users/gsharman/Documents/GitHub/detritalPy/example-data/ExampleDataset_1.xlsx',
			  '/Users/gsharman/Documents/GitHub/detritalPy/example-data/ExampleDataset_1.xlsx']

main_df, main_byid_df, samples_df, analyses_df = dFunc.loadDataExcel(dataToLoad)
```

Once data is loaded, samples can be selected either as a list of sample names

```python
sampleList = [(['POR-1','POR-2','POR-3','BUT-5','BUT-4','BUT-3','BUT-2','BUT-1'],'All')]
ages, errors, numGrains, labels = dFunc.sampleToData(sampleList, main_byid_df, sigma = '1sigma');
```

or as groups using a tuple structure.
```python
sampleList = [(['POR-1','POR-2','POR-3'],'Point of Rocks Sandstone'),
              (['BUT-5','BUT-4','BUT-3','BUT-2','BUT-1'],'Butano Sandstone')]
ages, errors, numGrains, labels = dFunc.sampleToData(sampleList, main_byid_df, sigma = '1sigma');
```

## Selected Examples

### Plot detrital age distributions
```python
fig = dFunc.plotAll(sampleList, ages, errors, numGrains, labels, x1=0, x2=300)
```
<img src="https://github.com/grsharman/detritalPy/blob/master/DZageDistributions.svg" width="900">

### Plot rim age versus core age
```python
sampleList = [(['11-Escanilla','12-Escanilla','10-Sobrarbe','7-Guaso','13-Guaso','5-Morillo','6-Morillo','14AB-M02','14AB-A04','14AB-A05','4-Ainsa','14AB-A06','15AB-352','15AB-118','15AB-150','3-Gerbe','14AB-G07','2-Arro','1-Fosado','14AB-F01'],'All Ainsa Basin')]    
ages, errors, numGrains, labels = dFunc.sampleToData(sampleList, main_byid_df, sigma = '1sigma');
rimsVsCores = dFunc.plotRimsVsCores(main_byid_df, sampleList, ages, errors, labels, x1=0, x2=3500, y1=0, y2=3500, plotLog=False, plotError=True, w=8, c=8)
```
<img src="https://github.com/grsharman/detritalPy/blob/master/rimVsCore.svg" width="600">

### Plot detrital age distributions in comparison to another variable (e.g., Th/U)
```python
figDouble = dFunc.plotDouble(sampleList, main_byid_df, ages, errors, numGrains, labels, variableName='Th_U', plotError=False, variableError=0.05, normPlots=False, plotKDE=False, colorKDE=False, colorKDEbyAge=False, plotPDP=True, colorPDP=False, colorPDPbyAge=True, plotHist=False, x1=0, x2=300, autoScaleY=False, y1=0, y2=2, b=5, bw=10, xdif=1, agebins=[0, 23, 65, 85, 100, 135, 200, 300, 500, 4500], agebinsc=['slategray','royalblue','gold','red','darkred','purple','navy','gray','saddlebrown'], w=10, t=3, l=1, plotLog=False, plotColorBar=False, plotMovingAverage=True, windowSize=25, KDElw=1, PDPlw=1);
```
<img src="https://github.com/grsharman/detritalPy/blob/master/doublePlot.svg" width="900">

### Multi-dimensional scaling
```python
figMDS, stress = dFunc.MDS(ages, errors, labels, sampleList, metric=False, plotWidth=10, plotHeight=8, plotPie=True, pieSize=0.05, agebins=[0, 23, 65, 85, 100, 135, 200, 300, 500, 4500], agebinsc=['slategray','royalblue','gold','red','darkred','purple','navy','gray','saddlebrown'], criteria='Dmax')
```
<img src="https://github.com/grsharman/detritalPy/blob/master/MDSplot.svg" width="600">

### (U-Th)/He vs U-Pb age "double dating" plot
```python
figDoubleDating = dFunc.plotDoubleDating(main_byid_df, sampleList, x1=0, x2=3500, y1=0, y2=500, plotKDE=False, colorKDE=False, colorKDEbyAge=True, plotPDP=True, colorPDP=True, colorPDPbyAge=False, plotHist=False, b=25, bw=10, xdif=1, width=10, height=10, savePlot=True, agebins=[0, 66, 180, 280, 310, 330, 410, 520, 700, 900, 1200, 1500, 3500], agebinsc=['olivedrab','purple','lightskyblue','lightseagreen','lightsteelblue','gold','sandybrown','orange','darkorange','firebrick','orchid','gray']);
```
<img src="https://github.com/grsharman/detritalPy/blob/master/doubleDating.svg" width="600">

## Related publications

If you find this code helpful in your research, please cite the accompanying article published in the Depositional Record.

Sharman GR, Sharman JP, Sylvester Z. detritalPy: A Python-based Toolset for Visualizing and Analyzing Detrital Geo-Thermochronologic Data. 2018. [https://doi.org/10.1002/dep2.45](https://doi.org/10.1002/dep2.45).

## License

detritalPy is licensed under the [Apache License 2.0](https://github.com/grsharman/detritalPy/blob/master/LICENSE.txt).
