# SD API Rework

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