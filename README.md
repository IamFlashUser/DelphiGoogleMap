﻿# DelphiGoogleMap  [![License](https://img.shields.io/badge/License-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)

**TEdgeGoogleMapViewer**: a Delphi "WebView2" Component to View Google Map with integrated Routing and Markers Support

### Actual official version 1.6.1 (VCL)

| Component | Description |
| - | - |
| ![google-maps-128.png](./Images/google-maps-128.png) | **TEdgeGoogleMapViewer** is a Delphi wrapper on "WebView2" Component that adds many Google Maps features like Routing and Markers Support (only for VCL). You can setup attributes of the components to see the Google Maps reacts to your changes. |

## New Setup for automatic installation of components

From 1.6.1 version a new "Installer" read-to-use is located in the Release area: [Download the Installer](https://github.com/EtheaDev/DelphiGoogleMap/releases/latest/download/DelphiGoogleMaps_Setup.exe).

The Installer automatically detect your Delphi versions, install sources, build and install packages and add source paths.

![Setup](./Images/Setup.png)

## Simple to use

It's very easy to use Google Maps API directly from Delphi Code, settings Component properties or invoking methods, for example:

```pascal
//Goto Address Example
EdgeGoogleMapViewer.GotoAddress(MemoAddress.Lines.Text);

//Goto Latitude/Longitude Example
var
  Location: TLatLng;
begin
  Location.Latitude := TEdgeGoogleMapViewer.TextToCoord(Latitude.Text);
  Location.Longitude := TEdgeGoogleMapViewer.TextToCoord(Longitude.Text);
  EdgeGoogleMapViewer.GotoLocation(Location);
  
//Setting Map View Mode
  EdgeGoogleMapViewer.MapTypeId := TGoogleMapTypeId(MapTypeIdComboBox.ItemIndex);
```

## How it works

The component works in a simple way: creates an html file with some javascript functions inside it and load it from disk, so the Delphi component can call those javascript functions inside the component methods, calling ExecuteScript(ScriptCommand) method of the TEdgeBrowser. If you want to analyse the content of the file created, put a break-point into TEdgeGoogleMapViewer.ShowMap at this line to know the name of the file created:

```pascal
NavigateToURL('file:///'+LFileName);
```
If you want to see how the component calls a javascript function inside the page, look for example at the method "GotoAddress":

```pascal
procedure TEdgeGoogleMapViewer.GotoAddress(const Address: string);
var
  ScriptCommand: String;
begin
  FAddress := Address;
  if FAddress = '' then
    HideMap
  else if not MapVisible then
    ShowMap(FAddress)
  else
  begin
    ScriptCommand := Format('codeAddress(%s)',[QuotedStr(ClearAddressText(Address))]);
    ExecuteScript(ScriptCommand);
  end;
end;
```
You can extend the component with any functionality you can execute at javascipt level, adding the function into the code, in the method "GetHTMLScript" that collect all the Javascript functions, or you can add your personal javascript code in the event "OnGetJavascript".

```pascal
function TEdgeGoogleMapViewer.GetHTMLScript : string;
var
  LJSScript : string;
begin
  LJSScript := GetJSVariables +sLineBreak+
  GetJSInitialize +sLineBreak+
  (...)
  GetJSRouteAddress + sLineBreak +
  GetJSCalcRoute;
  if Assigned(FOnGetJavascript) then
    FOnGetJavascript(Self,LJSScript);
  Result :=
  '<script type="text/javascript"> '+sLineBreak+
  LJSScript + sLineBreak +
  '</script> '+ sLineBreak;
end;
```

Look at the [GoogleAPI documentation](https://developers.google.com/maps/documentation/javascript?hl=it) if you need more functionality and please make a pull request to the project.
 
## A complete demo is available!

Notice that in the same folder of the executable you must place the correct webview2loader.dll (32 or 64 bit) as you can see into Demo\GoogleMaps\Bin\Win32 and Demo\GoogleMaps\Bin\Win64.

### Available for Delphi 10.4.2, Delphi 11 and Delphi 12 (VCL 32bit and 64bit platforms)

![Delphi Support](./Images/SupportingDelphi.jpg)

Related links: https://www.embarcadero.com/ - https://learndelphi.org/

## MANUAL INSTALLATION

If you don't want to use the installer, you can build the packages:

- Open {DelphiGoogleMapFolder}\Packages\{DelphiVer}\DelphiGoogleMapsPackages.groupproj from Delphi IDE
- Build Run-Time Package: DelphiGoogleMap
- Build and Install Design-Time Package:
- Add {DelphiGoogleMapFolder}\Source to Delphi source path

# The Demo in action

Open \Demo\GoogleMaps\GoogleMapsTest.dproj: Build (32 or 64 bit) and Run.

**Notice: you need a Google API key to activate the navigation**: by default the Demo request to input API before using the component, or you can edit TformMain.EdgeGoogleMapViewerBeforeShowMap event to add your API KEY to the component.

### Goto Address: Via S.Cecilia 4, Carugate, Milano

Input Address and click to "Goto Address":

![GoTo Address](./Images/GoToAddress.jpg)

### Goto Latitude/Longitude (25.767314 / -80.135694)

Input Latutude and Longitude and click to "Goto Latitude/Longitude"

![GoTo Latitude](./Images/GoToLatitudeLongitude.jpg)

### Route Bicycling - RoadMap View Style

Select Routing Mode "Bicycling", Select "RoadMap" View Mode, click to "Route by Location":

![Route Bicycling RoadMap](./Images/RouteBicyclingRoadMap.jpg)

### Route By Addresses - Hybrid and Traffic from Satellite View Style

Select Select "Hybrid" View Mode, Activate Traffic and click to "Route by Addreess":

![RouteByAddressHybridTraffic](./Images/RouteByAddressHybridTraffic.jpg)

### Route showing "Direction Panel"

Check "Direction Panel" and and click to "Route by Addresses":

![RouteWithDirectionPanel](./Images/RouteWithDirectionPanel.jpg)

### Showing "Print Panel" embedded into Browser

Click on "Show Print UI" button:

![ShowPrintUI](./Images/ShowPrintUI.jpg)

### Markers from Customer Table (customers.xml)

Load Customer Table, scroll down the table: for every record a Marker is added to the Map. Zoom out map to see all Markers.

![Markers Of Customers](./Images/MarkersOfCustomers.jpg)

### DOCUMENTATION

No documentation available, but the Demo explain very well the use of the component.

### RELEASE NOTES
12 Mar 2025 - Version 1.6.1
- Added support for Delphi 12 64bit IDE
- Added easy DelphiGoogleMaps_Setup.exe

24 Apr 2024 - Version 1.6.0
- Added functions to add Circles, Polylines or Polygons to map
- Added function ComputeDistanceBetween
- Enabled component into IDE using Win64 Platform

21 Aug 2023 - Version 1.5.0
- Changed Packages: separated Run-Time and Design-Time Packages
- Added "Show Print UI" Panel command
- Added support for Delphi 12

29 Nov 2022 - Version 1.4.0
- FullScreen Control showing switch
- Zoom Control showing switch
- MapType Control showing switch
- Update Demo to implement "fullscreen" request

15 Sep 2022 - Version 1.3.1
- Added 64bit platform to package and demo

09 Apr 2022 - Version 1.3.0
- Fixed problem building package for Delphi 10.4.2

07 Dec 2021 - Version 1.2.0
- Change base object to TCustomEdgeBrowser
- inherit properies to allow the hiding of OnWebMessageReceived and presenting new event OnWebUnhandledMessageReceived
- Map onclick, onzoom listener events
- New property OnMapClick and OnMapRightClick
- Demo to show right click menu and adding a marker
- Demo to have option on center on mouse click
- Demo updated to change zoom edit value on map zoom event
- GotoLocation changed to have option to place marker.

02 Nov 2021 - Version 1.1.0
- Added method to register UserDataFolder: TEdgeGoogleMapViewer.RegisterUserDataFolder
- Added Event Handler BeforeInitMap
- Added a secondary form to Demo (to test multiple instance of component)

24 Sep 2021 - Version 1.0.0
- Google Map Viewer Component based on TEdgeBrowser
- Full Demo available
