# glObjToJson

This MATLAB function `glObjToJson` takes a custom MATLAB GreenLight model object - `gl` as input and returns a JSON string representation of the object. 

## Table of Contents 
- [Installation](#installation) 
- [Usage](#usage) 
- [Functions](#functions)  
    - [glObjToJson](#globjtojson-1) 
    - [encodeNestedObj](#encodenestedobj) 
    - [encodeFieldValue](#encodefieldvalue)


## Installation 
1. Download the repository or copy the functions (`glObjToJson`, `encodeNestedObj`, and `encodeFieldValue`) to your project folder. 
2. Add the folder containing these functions to your MATLAB path using the `addpath` function:

```matlab
addpath('path/to/functions/folder');
```


## Usage

Convert a custom MATLAB object to a JSON string:

```matlab
json_str = glObjToJson(gl);
```



Where `gl` is an instance of the GreenLight model, To generate a `gl` instance using `GreenLight`, you can run the `runGreenLight.m` file located in the `runScenarios` folder of the [`GreenLight`](https://github.com/davkat1/GreenLight/blob/master/Code/runScenarios/runGreenLight.m)repository.


## Functions
### glObjToJson

```matlab
json_str = glObjToJson(gl);
```



Convert a custom MATLAB object "gl" to a JSON string.
#### Inputs: 
- `gl`: A MATLAB GreenLight model object that may have nested structures, instances of the DynamicModel or DynamicElement class, function handles, or other field types.
#### Outputs: 
- `json_str`: A JSON string representation of the input object `gl`, with all function handles in `gl` converted to strings.
### encodeNestedObj

```matlab
encodedObj = encodeNestedObj(obj);
```



Encode a nested MATLAB object into a new object.
#### Inputs: 
- `obj`: A MATLAB object that may have nested structures, instances of the DynamicModel or DynamicElement class, function handles, or other field types.
#### Outputs: 
- `encodedObj`: An encoded representation of the input object "obj".
### encodeFieldValue

```matlab
encodedValue = encodeFieldValue(fieldName, fieldValue);
```



Encode a field value based on its name and type.
#### Inputs: 
- `fieldName`: The name of the field to be encoded. 
- `fieldValue`: The value of the field to be encoded.
#### Outputs: 
- `encodedValue`: The encoded value of the input field value.


## Example

Consider a MATLAB object `gl` with nested structures, instances of custom classes, such as DynamicModel, DynamicElement, and function handles:

Calls the runGreenLight function from MATLAB with the specified lamp type, season, filename, parameters, crop maturity and return values.
```matlab
gl = runGreenLight(lampType, season, filename, paramNames, paramVals, isMature)
```


To convert this `gl` object to a JSON string, simply call the `glObjToJson` function:

```matlab
json_data = glObjToJson(gl)
```



The sample resulting JSON string `json_str` will be:

```json
{
   "x": {
      "co2Air": {
         "label": "x.co2Air",
         "def": "@(x,a,u,d,p)(1/(p.capCo2Air))*(a.mcBlowAir+a.mcExtAir+a.mcPadAir-(a.mcAirCan)-(a.mcAirTop)-(a.mcAirOut))",
         "val": [
            [
               0,
               811.57497020754624
            ],
            [
               300,
               811.75447291764658
            ]
         ]
      }
   },
   "a": {
      "tauShScrPar": {
         "label": "a.tauShScrPar",
         "def": "@(x,a,u,d,p)1-((u.shScr)*(1-(p.tauShScrPar)))",
         "val": [
            [
               0,
               1
            ],
            [
               300,
               1
            ]
         ]
      },
      "tauShScrPerPar":{
         "label":"a.tauShScrPerPar",
         "def":"@(x,a,u,d,p)1-((u.shScrPer)*(1-(p.tauShScrPerPar)))",
         "val":[
            [
               0,
               1
            ],
            [
               300,
               1
            ]
         ]
      },
   },
   "d": {
      "iGlob": {
         "label": "d.iGlob",
         "def": "@(x,a,u,d,p)d.iGlob",
         "val": [
            [
               0,
               0
            ],
            [
               300,
               0
            ]
         ]
      }
   },
   "p": {
      "alfaLeafAir": {
         "label": "p.alfaLeafAir",
         "def": "@(x,a,u,d,p)p.alfaLeafAir",
         "val": 5
      },
      "L": {
         "label": "p.L",
         "def": "@(x,a,u,d,p)p.L",
         "val": 2.45E+6
      },
      "sigma": {
         "label": "p.sigma",
         "def": "@(x,a,u,d,p)p.sigma",
         "val": 5.67E-8
      },
      "epsCan": {
         "label": "p.epsCan",
         "def": "@(x,a,u,d,p)p.epsCan",
         "val": 1
      }
   },
   "u": {
      "boil": {
         "label": "0+1.*(1./(1+exp(((-2./(p.tHeatBand)).*4.6052).*(x.tAir-(a.heatSetPoint)-((p.tHeatBand)/2)))))",
         "def": "@(x,a,u,d,p)0+1.*(1./(1+exp(((-2./(p.tHeatBand)).*4.6052).*(x.tAir-((max(((((p.lampsOn)~=(p.lampsOff)).*(((p.lampsOn)<(p.lampsOff)).*(min(max(0,min(1,24*(x.time-(floor(x.time)))-(p.lampsOn)+1)),max(0,min(1,p.lampsOff-(24*(x.time-(floor(x.time))))+1))))+(1-((p.lampsOn)<(p.lampsOff))).*(max(max(0,min(1,24*(x.time-(floor(x.time)))-(p.lampsOn)+1)),max(0,min(1,p.lampsOff-(24*(x.time-(floor(x.time))))+1)))))).*((d.dayRadSum)<(p.lampRadSumLimit))).*((((p.dayLampStart)<=(p.dayLampStop)).*(((p.dayLampStart)<(mod(x.time,365.2425)))&((mod(x.time,365.2425))<(p.dayLampStop)))+(1-((p.dayLampStart)<=(p.dayLampStop))).*(((p.dayLampStart)<(mod(x.time,365.2425)))|((mod(x.time,365.2425))<(p.dayLampStop)))).*1),d.isDay))*(p.tSpDay)+(1-(max(((((p.lampsOn)~=(p.lampsOff)).*(((p.lampsOn)<(p.lampsOff)).*(min(max(0,min(1,24*(x.time-(floor(x.time)))-(p.lampsOn)+1)),max(0,min(1,p.lampsOff-(24*(x.time-(floor(x.time))))+1))))+(1-((p.lampsOn)<(p.lampsOff))).*(max(max(0,min(1,24*(x.time-(floor(x.time)))-(p.lampsOn)+1)),max(0,min(1,p.lampsOff-(24*(x.time-(floor(x.time))))+1)))))).*((d.dayRadSum)<(p.lampRadSumLimit))).*((((p.dayLampStart)<=(p.dayLampStop)).*(((p.dayLampStart)<(mod(x.time,365.2425)))&((mod(x.time,365.2425))<(p.dayLampStop)))+(1-((p.dayLampStart)<=(p.dayLampStop))).*(((p.dayLampStart)<(mod(x.time,365.2425)))|((mod(x.time,365.2425))<(p.dayLampStop)))).*1),d.isDay)))*(p.tSpNight)+(p.heatCorrection)*((((1.*((d.iGlob)<(p.lampsOffSun))).*((d.dayRadSum)<(p.lampRadSumLimit))).*((((p.lampsOn)<=(p.lampsOff)).*(((p.lampsOn)<(24*(x.time-(floor(x.time)))))&((24*(x.time-(floor(x.time))))<(p.lampsOff)))+(1-((p.lampsOn)<=(p.lampsOff))).*(((p.lampsOn)<(24*(x.time-(floor(x.time)))))|((24*(x.time-(floor(x.time))))<(p.lampsOff)))).*1)).*((((p.dayLampStart)<=(p.dayLampStop)).*(((p.dayLampStart)<(mod(x.time,365.2425)))&((mod(x.time,365.2425))<(p.dayLampStop)))+(1-((p.dayLampStart)<=(p.dayLampStop))).*(((p.dayLampStart)<(mod(x.time,365.2425)))|((mod(x.time,365.2425))<(p.dayLampStop)))).*1)))-((p.tHeatBand)/2)))))",
         "val": [
            [
               0,
               0.0099006978376994809
            ],
            [
               300,
               0.99999982425931389
            ]
         ]
      }
   },
   "c": [],
   "g": [],
   "t": {
      "label": "10-Jan-2005 01:00:00",
      "def": [],
      "val": [
         0,
         300.00000223517418
      ]
   },
   "e": []
}
```



You can then save this JSON string to a file, or use it in other applications that work with JSON data.
## Limitations 
- The `glObjToJson` function is designed to work with GreenLight model MATLAB objects that have nested structures, instances of the `DynamicModel` or `DynamicElement` class, function handles, and other field types. However, it may not handle other possible MATLAB data types or custom classes. 
- The function currently assumes that function handles are only present in fields named 'def'. If you have function handles with different field names, you may need to modify the `encodeFieldValue` function accordingly.
- The function may not handle very large or complex objects efficiently.

## Contributing

If you have any suggestions, improvements, or bug reports, please create an issue or submit a pull request on the [GitHub repository](https://github.com/greenpeer/glObjToJson) . Your contributions are greatly appreciated!
