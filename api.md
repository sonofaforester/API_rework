# SD API Rework

## Some common changes
- Remove the HttpResponseException class, this was a carry over from .NET Framework and should be removed
- Return types should be of type ActionResult<T> [docs](https://docs.microsoft.com/en-us/aspnet/core/web-api/action-return-types?view=aspnetcore-2.2)
- Don't throw exceptions to return 404's and such. WebApi way of doing things it to return an IActionResult like so (these are defined in the ControllerBase class provided by the framework):
```sh
    return NotFound();
```
- When documenting APIs, don't get too verbose. It just creates noise and makes the swagger docs messy. If you are returning and actual object, than by all means include the typeof bits, otherwise they are just noise and redundant.
```sh
        [HttpPut("groups/{groupId}/monitoringpoint/type")]
        [ProducesResponseType(typeof(HttpResponseException), 400, Type = typeof(BadRequestResult))]
        [ProducesResponseType(typeof(HttpResponseException), 401, Type = typeof(UnauthorizedResult))]
        [ProducesResponseType(typeof(HttpResponseException), 404, Type = typeof(NotFoundResult))]
        [ProducesResponseType(typeof(HttpResponseException), 200, Type = typeof(OkResult))]
        public async Task<IActionResult> UpdateMonitoringPointType(Guid groupId, .... {
        ...
```
should just read like this:
```sh
        [HttpPut("groups/{groupId}/monitoringpoint/type")]
        [ProducesResponseType(400)]
        [ProducesResponseType(401)]
        [ProducesResponseType(404)]
        [ProducesResponseType(200)]
        public async Task<IActionResult> UpdateMonitoringPointType(Guid groupId, .... {
        ...
```
## Existing Indicator models


**IndicatorStateModel**
_Indicator State Model for logical layer_
```sh
{
    CurrentValue    number($double)     ## Current Value of indicator
    LastUpdate	    string($date-time)  ## Last time it was updated
    Id	            string($uuid)       ## Id of entity
    AlarmSeverity	integer($int32)     ## Alarm severity
}
```

**StateModel**
_State model for logical Layer_
```sh
{
    Id	            string($uuid)       ## Id of entity
    AlarmSeverity	integer($int32)     ## Alarm severity
}
```

**SimpleIndicatorModel**
```sh
{
    MetricTypeEnum	            string                          ## The metric type code of the given indicator
    Color	                    string                          ## The color the indicator uses in the legend
    AlarmDefinitionDto	        MetricAlarmDefinitionDto{...}
    LastDataProcessed	        string($date-time)              ## The last time the indicator processed data
    LastAlarmLogStateTypeCode	string                          ## The last alarm state the indicator was in (High warn/ High alarm/ …)
    DisplayUnits	            string                          ## The units to convert the points to when displaing on the trend
    BandLowFrequency	        number($double)                 ## The frequency of the low band
    BandHighFrequency	        number($double)                 ## The frequency of the high band
    HasSpectrum	                boolean                         ## True when the indicator may contain burst data
    BaseUnits	                string                          ## The units the data points are stored in
    DisplayUnitsType	        string                          ## A string to use for displaying the untis
    UnitPrecision	            integer($int32)                 ## The number of decimal places to store the data points in
    CurrentValue	            number($double)                 ## The current value of the indicator
    MetricType	                string                          ## The metric type code as a string
    VibrationUnits	            boolean                         ## Indicates whether vibration unis is velocity otherwise acceleration
    RoundedCurrentValue	        string                          ## The current value as a string rounded up.
    VibrationCalculation	    string                          ## The vibration value as a string
    IsVibration	                boolean                         ## True if the indicator recieves data from a vibration sensor
    IsDamagePressure	        boolean                         ## True if the indicator is of type DamageAccumulationPressure  
    FreqUnit	                string                          ## Hertz or CPM
    MetricStreamConstants	    [...]
    Baseline	                BaselineModel{...}
    BackCalculating	            boolean                         ## True if the indicator is currently having its data recalculated
    AlarmSeverityLevel	        number($double)                 ## A value between 0 and 10 for how close the current value is to a given alarm threshold
    NavigationIds	            string                          ## Ids for navigation
    Navigation	                string                          ## Non abbrevated Navigation
    NavigationFullName	        string                          ## Abbrevated Navigation
    MonitoringPointId	        string($uuid)                   ## The Id of the associated Monitoring Point
    AssetId	                    string($uuid)                   ## The Id of the associdated Asset if there is an asset in the parents
    AssignedSensorRoleType	    string                          ## Assigned sensor role type code for the indicator
    Name	                    string
    Id	                        string($uuid)
    Updated	                    string($date-time)
    Created	                    string($date-time)
    Active	                    boolean
}
```

**Alert Row Models**

