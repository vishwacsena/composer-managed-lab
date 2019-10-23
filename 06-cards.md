Update bot to come back with adaptive card
- Update relevant actions 
- Test scenrio works E2E

This rule for a thumbnail card works pretty well...

# WeatherCard(weather)
-```
[ThumbnailCard
    title = Weather for @{weather.city}
    text = @{DescribeWeather(weather}} and @{weather.temp}&deg;
    image = @{weather.icon}
]
```