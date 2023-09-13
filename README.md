# README
{{ template "hourly-table" $todayWeather.HourlyWeathers }}
{{ template "daily-table" .Weathers }}
{{ formatTime .UpdatedAt }}
name: "Cronjob"
on:
  schedule:
    - cron: '15 * * * *'

jobs:
  update-weather:
    permissions: write-all
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Generate README
        uses: huantt/weather-forecast@v1.0.5
        with:
          city: HaNoi
          days: 7
          weather-api-key: ${{ secrets.WEATHER_API_KEY }}
          template-file: 'README.md.template'
          out-file: 'README.md'
      - name: Commit
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git add .
          git commit -m "update weather"
          git push origin main
          