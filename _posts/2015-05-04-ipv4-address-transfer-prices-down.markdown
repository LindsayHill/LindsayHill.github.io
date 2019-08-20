---
author: lindsay
date: 2015-05-04 10:09:26+00:00
layout: post
slug: ipv4-address-transfer-prices-down
title: IPv4 Address Transfer Prices Down?
categories:
- Routing &amp; Switching
tags:
- APNIC
- IPv6
---

Last year I wrote about the [IPv4 Address Transfer Process]({% post_url 2014-02-26-ipv4-address-transfer-process %}). Recently I was involved in another IPv4 transfer. I was surprised to see that IPv4 prices have fallen in the last year. I have done some rudimentary analysis of the APNIC transfer statistics to try to figure out why.

APNIC publishes statistics on transfers at [ftp.apnic.net/public/transfers/apnic](ftp://ftp.apnic.net/public/transfers/apnic). These text files list all resource transfers that have taken place - the "to" &amp; "from" organisation, the resource type, the date, etc. I am very interested in looking at the trends. How many transactions take place each month, and how many addresses are being transferred?

I wrote a simple Python script to do this analysis for me. It retrieves the latest statistics, and converts them into a Google chart:

<script type="text/javascript" src="https://www.google.com/jsapi">
</script>

<script type="text/javascript">
   google.load("visualization", "1", {packages:["corechart"]});
</script>

<script type="text/javascript">
  var visualization;

  function drawVisualization() {
    var query = new google.visualization.Query(
      'https://docs.google.com/spreadsheets/d/1msKxmXbLdxSo84r_1h_SVRARMCkiFOZRuecS8OwNHBk/edit?usp=sharing&amp;headers=1');
    query.setQuery('select A, B, C');
  
    // Send the query with a callback function.
    query.send(handleQueryResponse);
  }
  
  function handleQueryResponse(response) {
    if (response.isError()) {
    alert('Error in query: ' + response.getMessage() + ' ' + response.getDetailedMessage());
    return;
    }
  
    var data = response.getDataTable();
    var options = {
      title: "APNIC Transfers per Month",
      height: 500,
      hAxis: {
        format: "MMM yyyy",
        gridlines: {count: "-1"},
      },
      seriesType: "bars",
      series: {
        0: {
          targetAxisIndex: 0,
          type: "line"
        },
        1: {
          targetAxisIndex: 1,
          type: "bars"
        },
      },
      vAxes: {
        0: {
          title: "Transactions",
          side: "left"
        },
        1: {
          title: "Prefixes (/24s)",
          side: "right"
        },
      },
    };
    visualization = new google.visualization.ComboChart(document.getElementById('visualization'));
    visualization.draw(data, options);
  }

  google.setOnLoadCallback(drawVisualization);
</script>

<div id="visualization">
</div>

Note this does not do live updates. It is a point in time snapshot, generated using the current data at the time the script is run. If you would like to update the code to do live updates, fork it from Github [here](https://github.com/LindsayHill/transfer-count). I'd also love to update the script to include stats from other RIRs.

The script graphs both the number of transactions per month, and the number of prefixes transferred per month (in /24s). You can see a clear up-tick in 2015. February 2015 was the highest month for prefixes transferred, and March and April set new records for transactions in a month.

Is this why prices have fallen? Now that we have well-established markets for IPv4 resource transfers, is that drawing more sellers onto the market? My current theory is that there are many businesses that have realised the value of the assets they hold, and are selling unused address space.

The tricky bit is trying to figure out where this is going. I believe that it won't take long before all the 'easy' sales have been done. If the space is lightly-used, or unused, it's easy to get rid of it. But those easy pickings will get shaken out, and I think we're going to see prices rising by this time next year. They'll go up for a few more years, then collapse once v6 is the norm.

Thoughts?

**UPDATE 20150705** - RIPE has published a more detailed breakdown of [transfer activity within their region](https://labs.ripe.net/Members/wilhelm/ipv4-transfers-in-the-ripe-ncc-service-region). They show a similar trend.
