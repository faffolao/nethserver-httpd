<template>
  <div>
    <h2>{{$t('dashboard.title')}}</h2>
    <div v-if=" !view.statsLoaded" class="spinner spinner-lg"></div>
    <div v-if="view.statsLoaded">
      <div>
        <h3>{{ $t('dashboard.apache_worker_status') }}</h3>
        <div class="row row-eq-height row-stat row-status container-fluid">
          <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
            <div v-if="Object.keys(status.statistics).length == 0" class="empty-piechart">
              <span class="fa fa-pie-chart"></span>
              <div>{{ $t('dashboard.empty_piechart_label') }}</div>
            </div>
            <div v-else id="apache-pie-chart"></div>
          </div>
        </div>
      </div>
      <div class="row">
        <div class="divider"></div>
        <h3>{{ $t('dashboard.apache_stats') }}</h3>
        <div class="row row-stat">
          <div class="row-inline-block">
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span class="card-pf-utilization-card-details-count stats-count">{{status.Uptime}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{status.UptimeUnit}} {{$t('dashboard.apache_uptime')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{status.TotalAccess}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.TotalAccess')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{status.Total_kbytes}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{status.Total_kbytesUnit}} {{$t('dashboard.Total_kbytes')}}</span>
              </span>
            </div>
          </div>
          <div class="row-inline-block">
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span class="card-pf-utilization-card-details-count stats-count">{{status.ReqPerSec}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.ReqPerSec')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{status.BytesPerSec}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.BytesPerSec')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{status.BytesPerReq}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.BytesPerReq')}}</span>
              </span>
            </div>
          </div>
        </div>
      </div>

      <div class="row">
        <div class="divider"></div>
        <h3>{{ $t('dashboard.informations') }}</h3>
        <div class="row row-stat">
          <div class="row-inline-block">
            <div
              v-if="live.packages.virtualhost"
              class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4"
            >
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{live.statistics.virtualhosts}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.number_virtualhosts')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{live.statistics.VhostReverse}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.number_VhostReverse')}}</span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{live.statistics.ProxyPass}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.number_ProxyPass')}}</span>
              </span>
            </div>
          </div>
          <div class="row-inline-block" v-if="live.packages.vsftpd">
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span class="font-size-140">
                <span>{{$t('dashboard.FTP_server_status')}}</span>
              </span>
              <span>
                {{live.services.vsftpd == 'enabled' ? $t('dashboard.FTP_enabled') : $t('dashboard.FTP_disabled') }}
                <span
                  :class="live.services.vsftpd == 'enabled'? 'fa fa-check green' : 'fa fa-times red'"
                ></span>
              </span>
            </div>
            <div class="stats-container col-xs-12 col-sm-6 col-md-4 col-lg-4">
              <span
                class="card-pf-utilization-card-details-count stats-count"
              >{{live.statistics.FTP}}</span>
              <span class="card-pf-utilization-card-details-description stats-description">
                <span
                  class="card-pf-utilization-card-details-line-2 stats-text"
                >{{$t('dashboard.number_FTP')}}</span>
              </span>
            </div>
          </div>
        </div>
      </div>

      <div class="divider"></div>

      <div class="row row-eq-height row-stat row-status container-fluid">
        <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
          <div class="panel panel-default">
            <div class="panel-heading">
              <h3 class="panel-title">{{ $t('dashboard.default_rpm_version') }}</h3>
            </div>
            <div class="panel-body">
              <span v-for="(item, key) in live.versions.default" :key="key">
                <span
                  class="card-pf-utilization-card-details-count stats-description-small col-xs-6"
                >{{key}}</span>
                <span
                  class="card-pf-utilization-card-details-description stats-description-small col-xs-6"
                >
                  <span class="card-pf-utilization-card-details-line-2 stats-text-small">{{ item }}</span>
                </span>
              </span>
            </div>
          </div>
        </div>

        <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
          <div class="panel panel-default">
            <div class="panel-heading">
              <h3 class="panel-title">{{ $t('dashboard.PHP_scl') }}</h3>
            </div>
            <div class="panel-body">
              <span v-if="Object.keys(live.versions.php_SCL).length == 0">
                <div
                  class="card-pf-utilization-card-details-count stats-description-small col-xs-6"
                >{{ $t('dashboard.no_installed_PHP') }}</div>
              </span>
              <span v-else v-for="(item, key) in live.versions.php_SCL" :key="key">
                <span
                  class="card-pf-utilization-card-details-count stats-description-small col-xs-6"
                >{{key}}</span>
                <span
                  class="card-pf-utilization-card-details-description stats-description-small col-xs-6"
                >
                  <span class="card-pf-utilization-card-details-line-2 stats-text-small">{{ item }}</span>
                </span>
              </span>
            </div>
          </div>
        </div>

        <div class="col-xs-12 col-sm-6 col-md-4 col-lg-4">
          <div class="panel panel-default">
            <div class="panel-heading">
              <h3 class="panel-title">{{ $t('dashboard.databases_scl') }}</h3>
            </div>
            <div class="panel-body">
              <span v-if="Object.keys(live.versions.database_SCL).length == 0">
                <div
                  class="card-pf-utilization-card-details-count stats-description-small col-xs-6"
                >{{ $t('dashboard.no_installed_database') }}</div>
              </span>
              <span v-else v-for="(item, key) in live.versions.database_SCL" :key="key">
                <span
                  class="card-pf-utilization-card-details-count stats-description-small col-xs-6"
                >{{key}}</span>
                <span
                  class="card-pf-utilization-card-details-description stats-description-small col-xs-6"
                >
                  <span class="card-pf-utilization-card-details-line-2 stats-text-small">{{ item }}</span>
                </span>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import generatePieChart from "@/piechart";
export default {
  name: "Dashboard",
  mounted() {
    this.getLive();
    this.getApacheStatus();
  },
  updated() {
    var $ = window.jQuery;
    $('[data-toggle="tooltip"]').tooltip();
    if (!this.apachePieChart) {
      this.apachePieChart = generatePieChart("#apache-pie-chart", {
        names: {
          SpareWorkers: this.$t("dashboard.SpareWorkers"),
          IdleWorkers: this.$t("dashboard.IdleWorkers"),
          RunningWorkers: this.$t("dashboard.RunningWorkers")
        },
        colors: {
          SpareWorkers: $.pfPaletteColors.green,
          IdleWorkers: $.pfPaletteColors.orange,
          RunningWorkers: $.pfPaletteColors.red
        },
        columns: []
      });
    }
    this.apachePieChart.load({
      json: this.status.statistics
    });
  },
  data() {
    return {
      view: {
        isLoaded: false,
        statsLoaded: false
      },
      live: {
        packages: {},
        statistics: {},
        services: {},
        versions: {
          database_SCL: {},
          php_SCL: {},
          default: {}
        }
      },
      status: {}
    };
  },
  methods: {
    getLive() {
      var context = this;

      context.view.statsLoaded = false;
      nethserver.exec(
        ["nethserver-httpd/dashboard/read"],
        {
          action: "live"
        },
        null,
        function(success) {
          try {
            success = JSON.parse(success);
          } catch (e) {
            console.error(e);
          }
          context.live = success;
          context.services =
            success.services && Object.keys(success.services).length > 0
              ? success.services
              : false;
          context.versions =
            success.versions && Object.keys(success.versions).length > 0
              ? success.versions
              : false;

          context.view.statsLoaded = true;
        },
        function(error) {
          console.error(error);
        },
        true //sudo
      );
    },
    getApacheStatus() {
      var context = this;

      context.view.isLoaded = false;
      nethserver.exec(
        ["nethserver-httpd/dashboard/read"],
        {
          action: "apacheStatus"
        },
        null,
        function(success) {
          try {
            success = JSON.parse(success);
          } catch (e) {
            console.error(e);
          }
          context.status = success;
          context.status.statistics = success.statistics;

          //use integer number
          context.status.ReqPerSec = parseFloat(
            Number(success.ReqPerSec).toFixed(0)
          );
          context.status.BytesPerSec = parseFloat(
            Number(success.BytesPerSec).toFixed(0)
          );
          context.status.BytesPerReq = parseFloat(
            Number(success.BytesPerReq).toFixed(0)
          );

          //display time units with integer
          if (Number(success.Uptime) < 3600) {
            context.status.Uptime = parseFloat(
              (Number(success.Uptime) / 60).toFixed(0)
            );
            context.status.UptimeUnit =
              " " + context.$i18n.t("dashboard.minute") + " ";
          } else if (Number(success.Uptime) < 86400) {
            context.status.Uptime = parseFloat(
              (Number(success.Uptime) / 3600).toFixed(0)
            );
            context.status.UptimeUnit =
              " " + context.$i18n.t("dashboard.hour") + " ";
          } else if (success.Uptime > 86400) {
            context.status.Uptime = parseFloat(
              (Number(success.Uptime) / 86400).toFixed(0)
            );
            context.status.UptimeUnit =
              " " + context.$i18n.t("dashboard.day") + " ";
          }

          //create bandwith units
          if (Number(success.Total_kbytes) < 1000) {
            context.status.Total_kbytes = parseFloat(
              (Number(success.Total_kbytes) / 1000).toFixed(2)
            );
            context.status.Total_kbytesUnit =
              " " + context.$i18n.t("dashboard.MB") + " ";
          } else if (Number(success.Total_kbytes) < 1000000) {
            context.status.Total_kbytes = parseFloat(
              (Number(success.Total_kbytes) / 1000000).toFixed(2)
            );
            context.status.Total_kbytesUnit =
              " " + context.$i18n.t("dashboard.GB") + " ";
          } else if (success.Total_kbytes > 1000000000) {
            context.status.Total_kbytes = parseFloat(
              (Number(success.Total_kbytes) / 1000000000).toFixed(2)
            );
            context.status.Total_kbytesUnit =
              " " + context.$i18n.t("dashboard.TB") + " ";
          }

          context.view.isLoaded = true;
        },
        function(error) {
          console.error(error);
        },
        true //sudo
      );
    }
  }
};
</script>

<style>
.empty-piechart {
  margin-top: 2em;
  text-align: center;
  color: #9c9c9c;
}
.empty-piechart .fa {
  font-size: 92px;
  color: #bbbbbb;
}

.divider {
  border-bottom: 1px solid #d1d1d1;
}

/* lamp-status */
.row-status {
  margin-left: -5px;
  margin-right: 0px;
}

/* panel */

.stats-description-small {
  float: left;
  overflow: hidden;
  text-overflow: ellipsis;
  font-size: 16px;
  font-weight: 300;
  line-height: 2;
}

.stats-text-small {
  line-height: 0.5;
}
.panel-body {
  flex-grow: 1;
}
.row-eq-height {
  display: -webkit-box;
  display: -webkit-flex;
  display: -ms-flexbox;
  display: flex;
  flex-flow: row wrap;
}
.row-eq-height > div {
  margin: 20px 0;
}
.row-eq-height .panel {
  min-height: 100%;
  display: flex;
  flex-direction: column;
}
.panel-body {
  flex-grow: 1;
}
.stats-count {
  font-size: 26px;
  font-weight: 300;
  margin-right: 10px;
  float: left;
  line-height: 1;
}
.mg-left-20 {
  margin-left: 20px !important;
}
.row {
  padding-left: 20px;
  padding-right: 20px;
}
.semi-bold {
  font-weight: 500;
}
.min-size {
  font-size: 18px;
}
.stats-text {
  margin-top: 10px !important;
  display: block;
}
.stats-description {
  float: left;
  line-height: 1;
}
.row-inline-block {
  display: inline-block;
  width: 100%;
}
.row-stat {
  margin-left: 0px;
  margin-right: 0px;
}

.font-size-140 {
  font-size: 140%;
}
.stats-container {
  padding: 20px !important;
  border-width: initial !important;
  border-style: none !important;
  border-color: initial !important;
  -o-border-image: initial !important;
  border-image: initial !important;
}

.stats-text {
  margin-top: 10px !important;
  display: block;
}

.stats-description {
  float: left;
  line-height: 1;
}

.stats-count {
  font-size: 26px;
  font-weight: 300;
  margin-right: 10px;
  float: left;
  line-height: 1;
}

.row-stat {
  margin-left: 0px;
  margin-right: 0px;
}

.compact {
  margin-bottom: 0px !important;
}

.small-list {
  padding-top: 5px;
  padding-bottom: 5px;
}

.no-mg-top {
  margin-top: 0px;
}
</style>
