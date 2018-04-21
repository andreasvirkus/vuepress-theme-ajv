<template>
  <main>
    <Content/>
    <!-- TODO: Use v-for to generate metrics placeholders -->
    <div class="dashboard-container" id="visitor-data">
      <h3>General visitor data</h3>

      <div class="row">
        <div class="col-md-8 col-xs-12">
          <div class="pageviews-interval"></div>
        </div>
        <div class="col-md-4 col-xs-12">
          <div class="total-pageviews"></div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="pageviews-by-day"></div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="popular-pages-visited"></div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="pageviews-by-browser"></div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="pageviews-by-device"></div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
            <div class="pageviews-by-country"></div>
        </div>
        <div class="col-md-6 col-xs-12">
            <div class="pageviews-by-city"></div>
        </div>
      </div>
    </div>

    <div class="dashboard-container" id="performance-data">
      <h3>Site's performance data based on last week</h3>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="response-time"></div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="avg-response"></div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="full-load-time">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="Site's load time in seconds">
                <span class="keen-dataviz-metric-value" style="padding-top: 97px;" id="full-load-time"></span>
                <span class="keen-dataviz-metric-title">full load time (s)</span>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="site-weight">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="Site weight in kB">
                <span class="keen-dataviz-metric-value" style="padding-top: 97px;" id="js-site-weight">WIP</span>
                <span class="keen-dataviz-metric-title">site weight (kB)</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="build-number">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="Number of times Metalsmith build has been run">
                <span class="keen-dataviz-metric-value" style="padding-top: 97px;">{{ buildNumber }}</span>
                <span class="keen-dataviz-metric-title">times built</span>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="latest-build">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="When the latest build took place">
                <span class="keen-dataviz-metric-value" style="padding-top: 97px; font-size: 1.6rem;">{{ date dateBuilt }}</span>
                <span class="keen-dataviz-metric-title">latest build</span>
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6 col-xs-12">
          <div class="latest-build">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="How long the latest build took">
                <span class="keen-dataviz-metric-value" style="padding-top: 97px; font-size: 1.6rem;">{{ format ( buildTime ) }}</span>
                <span class="keen-dataviz-metric-title">build duration (s)</span>
              </div>
            </div>
          </div>
        </div>
        <div class="col-md-6 col-xs-12">
          <div class="latest-build">
            <div class="keen-dataviz">
              <div class="keen-dataviz-metric metric-block" title="Something special">
                  <span class="keen-dataviz-metric-value" style="padding-top: 97px; font-size: 1.6rem;">WIP</span>
                  <span class="keen-dataviz-metric-title">Stay tuned</span>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<script src="/assets/dashboard/dashboard.js"></script>
<script src="https://d26b395fwzu5fz.cloudfront.net/keen-tracking-1.1.3.min.js"></script>
<script src="https://d26b395fwzu5fz.cloudfront.net/keen-dataviz-1.1.3.min.js"></script>
<script src="https://d26b395fwzu5fz.cloudfront.net/keen-analysis-1.2.2.js"></script>
<script>
  // TODO: Look if keen libs are on npm
  // TODO: Vuetify dashboard.js solution
  export default {
    data() {
      return {

      }
    },
    // Keen IO Settings
    created () {
      const timeframe = {
          start: "2017-06-03T07:00:00.000Z",
          end: new Date().toISOString()
      };

      window.renderVisitorData(new Keen({
          projectId: '5932a7c595cfc907a1f80c67',
          readKey: '03384FBE774B660D6C84C358DE6B68C0073053180D104FC19752DA666AB92D44032A5F8AE8F7AAE4E93DAAC8303A3C8FB4AEAD402FEC9B639164BC23953BEEC1D3893A48EB449673E429E5593BD22B0ABDA2A407360CF0EF1C7173E41CD03C1F'
      }), timeframe);

      window.renderPerformanceData(new Keen({
          projectId: '593d876495cfc907a1f8125c',
          readKey: 'B72287E07C5BBE0F25ADB1C392E3FD7B44C7C6A8397BCEA4401EEB343EEEE7C88DD5E7513387AE094E944652A2387C2DFA3CC930BD8256DB608A2B05A81EA1437A2AF6CB3D9B93B11CBECBD2199644A9CFFE493E2D4BBFA73541AF6EEAC813AB'
      }), timeframe);
    }
  }
</script>

<style src="https://d26b395fwzu5fz.cloudfront.net/keen-dataviz-1.1.3.min.css"></style>
<style src="//cdnjs.cloudflare.com/ajax/libs/flexboxgrid/6.3.1/flexboxgrid.min.css" type="text/css"></style>
<style>
  div[class^="col-"] {
      padding: 10px;
  }
  .keen-dataviz {
      background: #fff;
      border: 1px solid #e7e7e7;
      border-radius: 2px;
  }
  .keen-dataviz-title {
      border-bottom: 1px solid #e7e7e7;
      border-radius: 2px 2px 0 0;
      font-size: 13px;
      padding: 2px 10px 0;
      text-transform: uppercase;
  }
  .keen-dataviz-message {
      color: grey;
      display: block;
      font-size: 21px;
      font-weight: 200;
      line-height: 24px;
      text-align: center;
      box-sizing: border-box;
  }
  .keen-dataviz .keen-dataviz-table {
      overflow-x: auto;
  }

  .metric-block {
      background-color: rgb(0, 187, 222);
      width: auto;
      height: 300px;
  }
</style>