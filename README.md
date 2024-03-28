<h1> Ovarro LoggerVision Xilog API documentation </h1>
Questions? Find us at <a href=mailto:support.ld@ovarro.com> support.ld@ovarro.com </a>

<h1> Xilog Data Access </h1>
The Xilog API provides access to the raw data recorded by the Xilog logger. To use the API third parties must provide an authorization token with each request, which can be found <a href=https://atriumiot.com/accountmanagement>here</a>.

# Methods
#### Xilog Flow
- [*FlowLogger/All*](#flowloggeralltoken): Returns all loggers for the access token</li>
- [*FlowData/All*](#flowdataallidstartdateenddatetoken): Returns all data for logger for the specified date range</li>
- [*FlowData/Channel*](#flowdatachannelidindexstartdateenddatetoken): Returns all data for logger for the specified channel and date range</li>

#### Xilog NG
- [*Logger/All*](#loggeralltoken): Returns all loggers for the access token</li>
- [*Data/All*](#dataallserialnumberstartdateenddatetoken): Returns all data for logger for the specified date range</li>
- [*Data/Channel*](#datachannelserialnumberchannelnamestartdateenddatetoken): Returns all data for logger for the specified channel and date range</li>
- [*Data/DailyStats*](#datadailystatsserialnumberstartdateenddatetoken): Returns statistical data on each channel for the specified date range.</li>

#### Xilog+
- [*PlusLogger/All*](#plusloggeralltoken): Returns all loggers for the access token</li>
- [*PlusData*](#plusdataserialnumberchannelstartdateenddatetoken): Returns all data for logger channel for the specified date range</li>
- [*PlusData/ToUnits*](#plusdatatounitsserialnumberchannelstartdateenddateunittoken):Returns all data for logger channel for the specified date range, converted to a unit</li>
- [*PlusData/Meter*](#plusdatameterserialnumberchannelstartdateenddatetoken): Returns meter data for a channel for the specified date range.</li>
- [*PlusData/MinMax*](#plusdataminmaxserialnumberchannelstartdateenddatetoken): Returns statistical data on each channel for the specified date range.</li>
- [*DataTypes*](#xilog-datatypes): List of DataType enums.</li>
- [*UnitTypes*](#xilog-unittypes): List of UnitType enums.</li>

# API
## FlowLogger/All/{token}
#### Purpose
Returns array of loggers for the access token
#### Signature
<ol>
<li>Endpoint : https://xilogdataapi.atriumiot.com/FlowLogger/All
</li>
<li> Params </li>
  <ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>

</ol>

#### Return Value
An array of loggers containing id, serial number and name.
<pre>
[{
  id: string,
  serialNumber: string
  reference: string,
  channels: [{
    id: string,
    name: string,
    index: int,
    type: string,
    minTimestamp: string,
    maxTimestamp: string
  }],
  latitude: number,
  longitude: number,
  min: string,
  max: string,
  tags: [{
    name: string	
  }]
}]
</pre>

#### Example

https://xilogdataapi.atriumiot.com/FlowLogger/All/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
[
  {
    "id": "fc750aab-dd8e-4d6d-93fa-0e9a8a481706",
    "serialNumber": "123456789",
    "reference": "Test",
    "channels": [
      {
        "id": "6ad074df-a945-4a5d-9874-191062f2b257",
        "name": "Total Pulses A",
        "index": 1091,
        "type": "Total Pulses",
        "minTimestamp": "2023-12-16 07:51",
        "maxTimestamp": "2023-12-20 07:50"
      },
      {
        "id": "23f08689-07e4-4349-9fb0-6763ea78fc3d",
        "name": "Signal Strength (RSSI)",
        "index": 1014,
        "type": "Network Quality",
        "minTimestamp": "2023-12-16 07:51",
        "maxTimestamp": "2023-12-20 07:50"
      },
    ],
    "latitude": 50.8559393,
    "longitude": -0.98549994,
    "min": "2023-12-16 07:51",
    "max": "2023-12-20 07:50"
    tags: [{
      "name": "tag123"
    }]
  }
]
</pre>

## FlowData/All/{Id}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns a collection of the raw channel data for the specified Xilog Flow logger. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/FlowData/All/"> https://xilogdataapi.atriumiot.com/FlowData/All/</a>
</li>
<li> Params </li>
  <ul>
    <li>id: (string - required)</li>
    <ul>
      <li>Logger Id</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
An object which contains an array of channels each containing an array of the channel data:
<pre>
[{
  channel: {
    id: string,
    name: string,
    index: int,
    type: string,
    minTimestamp: string,
    maxTimestamp: string
  },
  data: [{
    timestamp: string,
    value: number
   }]
}]
</pre>

#### Example

https://xilogdataapi.atriumiot.com/FlowData/All/00000000-0000-0000-0000-000000000000/2023-12-12%2000:00/2023-12-19%2000:00/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
[
  {
    "channelName": {
        "id": "6ad074df-a945-4a5d-9874-191062f2b257",
        "name": "Total Pulses A",
        "index": 1091,
        "type": "Total Pulses",
        "minTimestamp": "2023-12-16 07:51",
        "maxTimestamp": "2023-12-20 07:50"
    },
    "data":[
      {
        "timestamp": "2023-12-16 07:51",
        "value": 488
      },
      {
        "timestamp": "2023-12-17 07:50",
        "value": 1167
      },
      {
        "timestamp": "2023-12-18 07:50",
        "value": 1931
      }
    ]
  },
  {
    "channel": {
      "id": "84094494-5177-4035-97f6-cc270e195fc4",
      "name": "Signal Strength (RSSI)",
      "index": 1014,
      "type": "Network Quality",
      "minTimestamp": "2023-12-16 07:51",
      "maxTimestamp": "2023-12-20 07:50"
    },
    "data":[
      {
        "timestamp": "2023-12-16 07:51",
        "value":13
      },
      {
        "timestamp": "2023-12-17 07:50",
        "value":9
      },
      {
        "timestamp": "2023-12-18 07:50",
        "value":16
      }
    ]
  }
]

</pre>

## FlowData/Channel/{Id}/{Index}/{StartDate}/{EndDate}/{Token})
#### Purpose
Returns a collection of the raw data for the specified Xilog Flow logger and channel index. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href=" https://xilogdataapi.atriumiot.com/FlowData/Channel/">https://xilogdataapi.atriumiot.com/FlowData/Channel/</a>
</li>
<li> Params </li>
  <ul>
    <li>Id: (string - required)</li>
    <ul>
      <li>Logger Id</li>
    </ul>
    <li>Index: (string - required)</li>
    <ul>
      <li>Index of the channel to retrieve data from.</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>
<h3>Return Value</h3>
An object which contains an array of the loggers channel data:
<pre>
{
  channel: {
    id: string,
    name: string,
    index: int,
    type: string,
    minTimestamp: string,
    maxTimestamp: string
  },
  data: [{
    timestamp: string,
    value: number
   }]
}
</pre>

#### Example

https://xilogdataapi.atriumiot.com/FlowData/Channel/00000000-0000-0000-0000-000000000000/1014/2023-12-12%2000:00/2023-12-19%2000:00/00000000-0000-0000-0000-000000000000

Example Output:

<pre>
[
  {
    "channel": {
      "id": "84094494-5177-4035-97f6-cc270e195fc4",
      "name": "Signal Strength (RSSI)",
      "index": 1014,
      "type": "Network Quality",
      "minTimestamp": "2023-12-16 07:51",
      "maxTimestamp": "2023-12-20 07:50"
    },
    "data":[
      {
        "timestamp": "2023-12-16 07:51",
        "value":13
      },
      {
        "timestamp": "2023-12-17 07:50",
        "value":9
      },
      {
        "timestamp": "2023-12-18 07:50",
        "value":16
      }
    ]
  }
]
</pre>

## Logger/All/{token}
#### Purpose
Returns array of loggers for the access token
#### Signature
<ol>
<li>Endpoint : https://xilogdataapi.atriumiot.com/Logger/All
</li>
<li> Params </li>
  <ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>

</ol>

#### Return Value
An array of loggers containing serial number and name.
<pre>
[{
  serial: number,
  name: string,
  channels: [string],
  location: {
    latitude: number,
    longitude: number
  },
  tags: [{
    name: string	
  }]
}]
</pre>

#### Example

https://xilogdataapi.atriumiot.com/Logger/All/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
[
  {
    "serial":12345678,
    "name":"ABCDEFGH",
    "channels": [
      "Pipe 1",
      "Flow 1"
    ],
    "location": {
      "latitude: 51.6456,
      "longitude: -1.6546
    },
    tags: []
  },
  {
    "serial":87654321,
    "name":"HGFEDCBA",
    "channels": [
      "Pipe",
      "Flow"
    ],
    "location": {
      "latitude: 51.6456,
      "longitude: -1.6546
    }
    "tags": [{
      "name": "tag123"
    }]
  }
]
</pre>

## Data/All/{SerialNumber}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns a collection of the raw channel data for the specified Xilog NG logger. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/Data/All/"> https://xilogdataapi.atriumiot.com/Data/All/</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (long - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
An object which contains an array of channels each containing an array of the channel data:
<pre>
[{
  channelName: string,
  data: [{
     type: LogType,
     data: [{
        timestamp: string,
        value: number
     }]
   }]
}]
</pre>
LogTypes:
<ul>
<li> Main = 8 </li>
<li> Min = 4 </li>
<li> Max = 2 </li>
<li> Standard Deviation = 1 </li> 
</ul>

#### Example

https://xilogdataapi.atriumiot.com/Data/All/12345678/2021-08-05%2003:30/2021-08-05%2003:40/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
[
  {
    "channelName":"Flow 1",
    "data":[
      {
        "type":8,
        "data":[
          {
            "timestamp":"2021-08-05 03:30:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:31:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:32:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:38:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:39:30",
            "value":0
          }
        ]
      }
    ]
  },
  {
    "channelName":"Flow 2",
    "data":[
      {
        "type":8,
	      "data":[
          {
            "timestamp":"2021-08-05 03:30:30",
            "value":0 
          },
          {
            "timestamp":"2021-08-05 03:39:30",
            "value":0
          }
        ]
      }
    ]
  }
]

</pre>

## Data/Channel/{SerialNumber}/{ChannelName}/{StartDate}/{EndDate}/{Token})
#### Purpose
Returns a collection of the raw data for the specified Xilog NG logger and channel name. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href=" https://xilogdataapi.atriumiot.com/Data/Channel/">https://xilogdataapi.atriumiot.com/Data/Channel/</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (long - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>ChannelName: (string - required)</li>
    <ul>
      <li>Name of the channel to retrieve data from.</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>
<h3>Return Value</h3>
An object which contains an array of the loggers channel data:
<pre>
{
  channelName: string,
  data: [{
     type: LogType,
     data: [{
      timestamp: string,
      value: number
     }]
   }]
}
</pre>

#### Example

https://xilogdataapi.atriumiot.com/Data/Channel/12345678/Flow%201/2021-08-05%2003:30/2021-08-05%2003:40/00000000-0000-0000-0000-000000000000

Example Output:

<pre>
[
  {
    "channelName":"Flow 1",
    "data":[
      {
        "type":8,
	      "data":[
	        {
            "timestamp":"2021-08-05 03:30:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:31:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:32:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:38:30",
            "value":0
          },
          {
            "timestamp":"2021-08-05 03:39:30",
            "value":0
          }
        ]
      }
    ]
  }
]
</pre>

## Data/DailyStats/{SerialNumber}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns the daily data statistics for the serial number and specified date. 
#### Signature
<ol>
<li>Endpoint : <a href=" https://xilogdataapi.atriumiot.com/Data/DailiyStats/">  https://xilogdataapi.atriumiot.com/Data/DailiyStats/</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (long - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>

</ol>

#### Return Value
An object containing meta data for channel:
<pre>
{
  channelName: string,
  dailyStats: [{
    date: datetime,
    minTime: dateTime,
    maxTime: dateTime,
    minValue: number,
    maxValue: number,
    totaliser: number
  }]
}

</pre>
#### Example

https://xilogdataapi.atriumiot.com/Data/DailyStats/12345678/2021-08-15/2021-08-16/00000000-0000-0000-0000-000000000000

<pre>
{
  "channelName": "Flow 1",
  "dailyStats": [
    {
      "date": "2021-08-15 00:00:00",
      "minTime": "2021-08-15 00:01:00",
      "maxTime": "2021-08-15 00:00:00",
      "minValue": 0,
      "maxValue": 0,
      "totaliser": 0
    },
    {
      "date": "2021-08-16 00:00:00",
      "minTime": "2021-08-16 00:01:00",
      "maxTime": "2021-08-16 00:00:00",
      "minValue": 0,
      "maxValue": 0,
      "totaliser": 0
    }
  ],
}

</pre>

## PlusLogger/All/{token}
#### Purpose
Returns array of loggers for the access token
#### Signature
<ol>
<li>Endpoint : https://xilogdataapi.atriumiot.com/PlusLogger/All
</li>
<li> Params </li>
  <ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>

</ol>

#### Return Value
An array of loggers containing serial number and name.
<pre>
[{
  serial: number,
  name: string,
  channels: [
    {
      index: string,
      readingType: string,
      dataType: int,
      UnitType: int
    }
  ],
  location: {
    latitude: number,
    longitude: number
  }
}]
</pre>

#### Example

https://xilogdataapi.atriumiot.com/PlusLogger/All/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
[
  {
    "serial":12345678,
    "name":"ABCDEFGH",
    "channels": [
      {
        "index": "d1a",
        "readingtype": "Average",
        "dataType": 1,
        "unitType": 8
      }
    ],
    "location": {
      "latitude: 51.6456,
      "longitude: -1.6546
    }
  },
  {
    "serial":12345678,
    "name":"ABCDEFGH",
    "channels": [
      {
        "index": "d1a",
        "readingtype": "Average",
        "dataType": 1,
        "unitType": 8
      },
      {
        "index": "a1",
        "readingtype": "Average",
        "dataType": 2,
        "unitType": 4
      }
    ],
    "location": {
      "latitude: 51.6456,
      "longitude: -1.6546
    }
  },
]
</pre>

## PlusData/{SerialNumber}/{Channel}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns the channel data for the specified Xilog+ logger. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/PlusData/"> https://xilogdataapi.atriumiot.com/PlusData/</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (int - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>Channel: (string - required)</li>
    <ul>
      <li>Channel index</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
Channel object with an array of the channel data:
<pre>
{
  channel: {
    index: string,
    readingType: string,
    dataType: int,
    UnitType: int
  },
  data: [
    {
      timestamp: string,
      value: number
    }
  ]
}

</pre>

#### Example

https://xilogdataapi.atriumiot.com/PlusData/12345678/d1a/2021-08-05%2003:30/2021-08-05%2003:40/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
{
  "channel": {
    {
      "index": "d1a",
      "readingtype": "Average",
      "dataType": 1,
      "unitType": 8
    }
  },
  "data": [
    {
      "timestamp":"2021-08-05 03:30:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:31:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:32:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:38:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:39:30",
      "value":0
    }
  ]
}

</pre>

## PlusData/ToUnits/{SerialNumber}/{Channel}/{StartDate}/{EndDate}/{unit}/{Token}
#### Purpose
Returns the channel data for the specified Xilog+ logger, converted to a unit. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/PlusData/ToUnits"> https://xilogdataapi.atriumiot.com/PlusData/ToUnits/</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (int - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>Channel: (string - required)</li>
    <ul>
      <li>Channel index</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
     <li>Unit: (int - required)</li>
    <ul>
      <li>Unit enum to be converted to</li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
Channel object with an array of the channel data:
<pre>
{
  channel: {
    index: string,
    readingType: string,
    dataType: int,
    UnitType: int
  },
  data: [
    {
      timestamp: string,
      value: number
    }
  ]
}
</pre>

#### Example

https://xilogdataapi.atriumiot.com/PlusData/ToUnits/12345678/d1a/2021-08-05%2003:30/2021-08-05%2003:40/1/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
{
  "channel": {
    {
      "index": "d1a",
      "readingtype": "Average",
      "dataType": 1,
      "unitType": 8
    }
  },
  "data": [
    {
      "timestamp":"2021-08-05 03:30:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:31:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:32:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:38:30",
      "value":0
    },
    {
      "timestamp":"2021-08-05 03:39:30",
      "value":0
    }
  ]
}

</pre>

## PlusData/Meter/{SerialNumber}/{Channel}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns the meter channel data for the specified Xilog+ logger. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/PlusData/Meter"> https://xilogdataapi.atriumiot.com/PlusData/Meter</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (int - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>Channel: (string - required)</li>
    <ul>
      <li>Channel index</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
Channel object with an array of the channel data:
<pre>
{
  channel: {
    index: string,
    readingType: string,
    dataType: int,
    UnitType: int
  },
  data: [
    {
      timestamp: string,
      value: number
    }
  ]
}
</pre>

#### Example

https://xilogdataapi.atriumiot.com/PlusData/Meter/12345678/d1a/2021-08-05/2021-08-09/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
{
  "channel": {
    {
      "index": "d1a",
      "readingtype": "Total",
      "dataType": 1,
      "unitType": 8
    }
  },
  "data": [
    {
      "timestamp":"2021-08-05 00:00:00",
      "value":0
    },
    {
      "timestamp":"2021-08-06 00:00:00",
      "value":0
    },
    {
      "timestamp":"2021-08-07 00:00:00",
      "value":0
    },
    {
      "timestamp":"2021-08-08 00:00:00",
      "value":0
    }
  ]
}

</pre>

## PlusData/MinMax/{SerialNumber}/{Channel}/{StartDate}/{EndDate}/{Token}
#### Purpose
Returns the min and max channel data for the specified Xilog+ logger. (Max 8 days)
#### Signature
<ol>
<li>Endpoint : <a href="https://xilogdataapi.atriumiot.com/PlusData/MinMax"> https://xilogdataapi.atriumiot.com/PlusData/minMax</a>
</li>
<li> Params </li>
  <ul>
    <li>SerialNumber: (int - required)</li>
    <ul>
      <li>Logger Serial number as displayed on device</li>
    </ul>
    <li>Channel: (string - required)</li>
    <ul>
      <li>Channel index</li>
    </ul>
    <li>StartDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to start querying loggers channel data</li>
    </ul>
    <li>EndDate: (string - yyyy-MM-dd HH:mm)</li>
    <ul>
      <li>Date at which to finish querying loggers channel data. (No more than 8 days after start date) </li>
    </ul>
    <li>Token: (string - required)</li>
    <ul>
      <li>Access token</li>
    </ul>
  </ul>
</ol>

#### Return Value
Channel object with the min and max data:
<pre>
{
  channel: {
    index: string,
    readingType: string,
    dataType: int,
    UnitType: int
  },
  data: [
    {
      timestamp: string,
      value: number
    }
  ]
}
</pre>

#### Example

https://xilogdataapi.atriumiot.com/PlusData/MinMax/12345678/d1a/2021-08-05%2003:30/2021-08-08%2003:40/00000000-0000-0000-0000-000000000000

Example Output:
<pre>
{
  "channel": {
    {
      "index": "d1a",
      "readingtype": "Average",
      "dataType": 1,
      "unitType": 8
    }
  },
  "data": [
    {
      "timestamp":"2021-08-05 03:30:30",
      "value":-1
    },
    {
      "timestamp":"2021-08-07 09:15:30",
      "value":1
    }
  ]
}

</pre>

# Xilog+ DataTypes
<pre>
{
  None = 0,
  Flow = 1,
  Pressure = 2,
  Depth = 3,
  Volume = 4,
  Chlorine = 5,
  Colour = 6,
  Turbidity = 7,
  Temperature = 8,
  Rainfall = 9,
  Conductivity = 10,
  REDOX = 11,
  pH = 12,
  DissolvedOxygen = 13,
  Height = 14,
  ChangeOfState = 15,
  Velocity = 16,
  UVEnergy = 19,
  UVIntensity = 20,
}
</pre>

# Xilog+ UnitTypes
<pre>
{
  None = 0,
  LitresPerSecond = 1,
  LitresPerHour = 2,
  CubicMetresPerHour = 3,
  Metres = 4,
  Bar = 5,
  MilliMetres = 6,
  MilliBar = 7,
  CubicMetres = 8,
  Litres = 9,
  MilliGramsPerLitre = 10,
  Hazen = 11,
  NTU = 12,
  DegreesCentigrade = 13,
  DegreesFarenheit = 14,
  DegreesKelvin = 15,
  MilliMetresPerHour = 16,
  MilliMetresPerDay = 17,
  MicroSiemens = 18,
  MilliSiemens = 19,
  Volts = 20,
  CubicMetresPerSecond = 21,
  MegaLitresPerDay = 22,
  USGallonsPerMinute = 23,
  USGallonsPerHour = 24,
  USMegaGallonsPerDay = 25,
  USGallons = 26,
  PoundsPerSquareInch = 29,
  Unitless = 30,
  Centimetres = 31,
  OnOff = 32,
  MilliAmps = 33,
  USGallonsPerSecond = 34,
  USGallonsPerDay = 35,
  CubicFeetPerSecond = 36,
  CubicFeetPerMinute = 37,
  CubicFeetPerHour = 38,
  CubicFeetPerDay = 39,
  CubicFeet = 40,
  USMegaGallons = 41,
  Feet = 42,
  Inches = 43,
  MilliMetresPerSecond = 44,
  MetresPerSecond = 45,
  FeetPerSecond = 46,
  InchesPerSecond = 47,
  LitresPerMinute = 48,
  KilowattHour = 49,
  Kilowatt = 50,
  Joules = 51,
  JoulesPerSecond = 52,
  Watt = 53,
  WattHour = 54,
  MilliWattsPerCentimetreSquared = 55,
  MilliJoulesPerCentimetreSquared = 56,
  KilogramsPerCentimetreSquared = 57,
  MegawattHour = 59,
  MilliVolts = 60,
  Kilopascals = 61,
  Megapascals = 62,
  Decibels = 63,
}
</pre>
