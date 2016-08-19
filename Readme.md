﻿[![Build status](https://ci.appveyor.com/api/projects/status/9oix5m8lgqybda72?svg=true)](https://ci.appveyor.com/project/Stratajet/unit-conversion)
[![NuGet](https://img.shields.io/nuget/v/UnitConversion.svg?maxAge=2592000)](https://www.nuget.org/packages/UnitConversion)
[![MIT License](https://img.shields.io/github/license/Stratajet/UnitConversion.svg)](https://raw.githubusercontent.com/Stratajet/UnitConversion/master/LICENSE)

### Unit Conversion

An expansible dotnet class library intended to fill the gap of a converter library which supports all modern dotnet platforms
* .Net Framework 4.0+
* .Net Core

UnitConversion is designed for programmatic use, and to be both expansible and flexible to use with custom business logic

***
##### Example

```C#
// Simple programmatic approach to conversion between units
var converter = new MassConverter("kg", "lbs");
double kg = 452;
double lbs = converter.LeftToRight(kg);
Console.WriteLine("452kg in pounds is " + lbs);

// Converting both ways is easy
double originalKgs = converter.RightToLeft(lbs);
Console.WriteLine("Converted back: " + originalKgs);

// You may also want a simpler level of precision
lbs = converter.LeftToRight(kg, 2);
Console.WriteLine("452kg in pounds (to 2 decimal places) is " + lbs);

// You can easily customise converters to support synonyms used in business logic, such as those stored on a database
string myUnitName = "MTOW (KG)";
kg = 3000;
converter.AddSynonym("kg", myUnitName);
converter.UnitLeft = myUnitName;
lbs = converter.LeftToRight(kg);
Console.WriteLine("3000 MTOW (KG) in lbs is " + lbs);

// Add a new unit with a custom conversion factor
converter.AddUnit("Chuck Norris", 9001);
converter.UnitRight = "Chuck Norris";
kg = 7;
var chucks = converter.LeftToRight(kg);
Console.WriteLine("7kg is equal to " + lbs + " chucks");

Console.Read();
```

****
##### Converters are easy to define and contribute to
```C#
    public class DistanceConverter : BaseUnitConverter {
        UnitFactors units = new UnitFactors("m") {
            { new UnitFactorSynonyms("m", "metre"), 1 },
            { new UnitFactorSynonyms("km", "kilometre"), 0.001 },
            { new UnitFactorSynonyms("cm", "centimetre"), 100 },
            { new UnitFactorSynonyms("mm", "millimetre"), 1000 },
            { new UnitFactorSynonyms("ft", "foot", "feet"), 1250d / 381 },
            { new UnitFactorSynonyms("yd", "yard"), 1250d / 1143 },
            { "mile", 125d / 201168 },
            { new UnitFactorSynonyms("in", "inch"), 5000d / 127 },
        };

        public DistanceConverter(string leftUnit, string rightUnit) {
            Instantiate(units, leftUnit, rightUnit);
        }
        public DistanceConverter() {
            Instantiate(units);
        }
    }
```