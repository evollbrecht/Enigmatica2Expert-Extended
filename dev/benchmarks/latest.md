## Minecraft load time benchmark

---

<p align="center" style="font-size:160%;">
MC total load time:<br>
309 sec
<br>
<sup><sub>(
5:09 min
)</sub></sup>
</p>

<br>
<!--
Note for image scripts:
  - Newlines are ignored
  - This characters cant be used: +<"%#
-->
<p align="center">
<img src="https://quickchart.io/chart.png?w=400&h=60&c={
  type: 'horizontalBar',
  data: {
    datasets: [
        {label: 'Mixins\n', data: [35.00]},
        {label: 'Construction\n', data: [63.00]},
        {label: 'PreInit\n', data: [142.00]},
        {label: 'Init\n', data: [67.00]},
    ]
  },
  options: {
    layout: { padding: { top: 10 } },
    scales: {
      xAxes: [{display: false, stacked: true}],
      yAxes: [{display: false, stacked: true}],
    },
    elements: {rectangle: {borderWidth: 2}},
    legend: {display: false},
    plugins: {datalabels: {
      color: 'white',
      font: {
        family: 'Consolas',
      },
      formatter: (value, context) =>
        [context.dataset.label, value, 's'].join('')
    }},
    annotation: {
      clip: false,
      annotations: [{
          type: 'line',
          scaleID: 'x-axis-0',
          value: 35,
          borderColor: 'black',
          label: {
            content: 'Window appear',
            fontSize: 8,
            enabled: true,
            xPadding: 8, yPadding: 2,
            yAdjust: -20
          },
        }
      ]
    },
  }
}"/>
</p>

<br>

# Mods Loading Time

<p align="center">
<img src="https://quickchart.io/chart.png?w=400&h=300&c={
  type: 'outlabeledPie',
  options: {
    rotation: Math.PI,
    cutoutPercentage: 25,
    plugins: {
      legend: !1,
      outlabels: {
        stretch: 5,
        padding: 1,
        text: (v,i)=>[
          v.labels[v.dataIndex],' ',
          (v.percent*1000|0)/10,
          String.fromCharCode(37)].join('')
      }
    }
  },
  data: {...
`
436e17  9.79s Had Enough Items;
395E14  1.43s [JEI Ingredient Filter];
395E14  9.72s [JEI Plugins];
516fa8  9.07s Ender IO;
5161a8  7.87s CraftTweaker2;
6e5e17  7.63s Tinkers' Antique;
5E5014  6.00s [TCon Textures];
8f304e  6.99s Astral Sorcery;
a651a8  5.41s IndustrialCraft 2;
cd922c  3.57s NuclearCraft;
813e81  3.38s OpenComputers;
6e175e  3.23s Recurrent Complex;
213664  3.21s Forestry;
306e8f  2.52s Custom Loading Screen;
308f7e  2.48s Quark: RotN Edition;
8c2ccd  2.43s Immersive Engineering;
ba3eb8  2.42s Cyclic;
436e17  2.37s Integrated Dynamics;
216364  2.35s Thermal Expansion;
444444 37.88s 25 Other mods;
333333 49.01s 155 'Fast' mods (1.0s - 0.1s);
222222  7.93s 295 'Instant' mods (%3C 0.1s)
`
    .split(';').reduce((a, l) => {
      l.match(/(\w{6}) *(\d*\.\d*) ?s (.*)/s)
      .slice(1).map((a, i) => [[String.fromCharCode(35),a].join(''), a,
        a.length > 15 ? a.split(/(?%3C=.{9})\s(?=\S{5})/).join('\n') : a
      ][i])
      .forEach((s, i) =>
        [a.datasets[0].backgroundColor, a.datasets[0].data, a.labels][i].push(s)
      );
      return a
    }, {
      labels: [],
      datasets: [{
        backgroundColor: [],
        data: [],
        borderColor: 'rgba(22,22,22,0.3)',
        borderWidth: 1
      }]
    })
  }
}"/>
</p>

<br>

# Loader steps

Show how much time each mod takes on each game load phase.

JEI/HEI not included, since its load time based on other mods and overal item count.

<p align="center">
<img src="https://quickchart.io/chart.png?w=400&h=450&c={
  options: {
    scales: {
      xAxes: [{stacked: true}],
      yAxes: [{stacked: true}],
    },
    plugins: {
      datalabels: {
        anchor: 'end',
        align: 'top',
        color: 'white',
        backgroundColor: 'rgba(46, 140, 171, 0.6)',
        borderColor: 'rgba(41, 168, 194, 1.0)',
        borderWidth: 0.5,
        borderRadius: 3,
        padding: 0,
        font: {size:10},
        formatter: (v,ctx) =>
          ctx.datasetIndex!=ctx.chart.data.datasets.length-1 ? null
            : [((ctx.chart.data.datasets.reduce((a,b)=>a- -b.data[ctx.dataIndex],0)*10)|0)/10,'s'].join('')
      },
      colorschemes: {
        scheme: 'office.Damask6'
      }
    }
  },
  type: 'bar',
  data: {...(() => {
    let a = { labels: [], datasets: [] };
`
0: Construction;
1: Loading Resources;
2: PreInitialization;
3: Initialization;
4: InterModComms;
5: LoadComplete;
6: ModIdMapping;
7: Other
`
    .split(';')
      .map(l => l.match(/\d: (.*)/).slice(1))
      .forEach(([name]) => a.datasets.push({ label: name, data: [] }));
`
                                  0      1      2      3      4      5      6      7;
Ender IO                      | 1.60| 0.01| 2.82| 0.29| 4.34| 0.00| 0.01| 0.00;
CraftTweaker2                 | 0.09| 0.00| 2.59| 5.15| 0.00| 0.04| 0.00| 0.00;
Tinkers' Antique              | 0.79| 0.01| 0.14| 0.70| 0.00| 0.00| 0.00| 6.00;
Astral Sorcery                | 0.16| 0.00| 5.13| 1.70| 0.00| 0.00| 0.00| 0.00;
IndustrialCraft 2             | 1.08| 0.01| 3.52| 0.81| 0.00| 0.00| 0.00| 0.00;
NuclearCraft                  | 0.05| 0.01| 2.69| 0.80| 0.00| 0.00| 0.03| 0.00;
OpenComputers                 | 0.14| 0.01| 1.58| 1.59| 0.07| 0.00| 0.00| 0.00;
Recurrent Complex             | 0.16| 0.00| 0.31| 2.76| 0.00| 0.00| 0.00| 0.00;
Forestry                      | 0.42| 0.01| 2.01| 0.77| 0.00| 0.00| 0.00| 0.00;
Custom Loading Screen         | 2.52| 0.00| 0.00| 0.00| 0.00| 0.00| 0.00| 0.00;
[Mod Average]                 | 0.07| 0.00| 0.16| 0.08| 0.01| 0.01| 0.00| 0.01
`
    .split(';').slice(1)
      .map(l => l.split('|').map(s => s.trim()))
      .forEach(([name, ...arr], i) => {
        a.labels.push(name);
        arr.forEach((v, j) => a.datasets[j].data[i] = v)
      }); return a
  })()}
}"/>
</p>

<br>

# TOP JEI Registered Plugis

<p align="center">
<img src="https://quickchart.io/chart.png?w=500&h=200&c={
  options: {
    elements: { rectangle: { borderWidth: 1 } },
    legend: false,
    scales: {
      yAxes: [{ ticks: { fontSize: 9, fontFamily: 'Verdana' }}],
    },
  },
  type: 'horizontalBar',
    data: {...(() => {
      let a = {
        labels: [], datasets: [{
          backgroundColor: 'rgba(0, 99, 132, 0.5)',
          borderColor: 'rgb(0, 99, 132)',
          data: []
        }]
      };
`
 1.47: li.cil.oc.integration.jei.ModPluginOpenComputers;
 1.29: crazypants.enderio.base.integration.jei.JeiPlugin;
 0.87: com.buuz135.industrial.jei.JEICustomPlugin;
 0.87: jeresources.jei.JEIConfig;
 0.78: com.rwtema.extrautils2.crafting.jei.XUJEIPlugin;
 0.56: mezz.jei.plugins.vanilla.VanillaPlugin;
 0.50: crazypants.enderio.machines.integration.jei.MachinesPlugin;
 0.39: ic2.jeiIntegration.SubModule;
 0.37: nc.integration.jei.NCJEI;
 0.22: cofh.thermalexpansion.plugins.jei.JEIPluginTE;
 0.20: com.buuz135.thaumicjei.ThaumcraftJEIPlugin;
 0.19: knightminer.tcomplement.plugin.jei.JEIPlugin;
 2.01: Other
`
        .split(';')
        .map(l => l.split(':'))
        .forEach(([time, name]) => {
          a.labels.push(name);
          a.datasets[0].data.push(time)
        })
        ; return a
    })()
  }
}"/>
</p>

<br>

# FML Stuff

Loading bars that usually not related to specific mods.

⚠️ Shows only steps that took 1.0 sec or more.

<p align="center">
<img src="https://quickchart.io/chart.png?w=500&h=400&c={
  options: {
    rotation: Math.PI*1.125,
    cutoutPercentage: 55,
    plugins: {
      legend: !1,
      outlabels: {
        stretch: 5,
        padding: 1,
        text: (v)=>v.labels
      },
      doughnutlabel: {
        labels: [
          {
            text: 'FML stuff:',
            color: 'rgba(128, 128, 128, 0.5)',
            font: {size: 18}
          },
          {
            text: '122.31s',
            color: 'rgba(128, 128, 128, 1)',
            font: {size: 22}
          }
        ]
      },
    }
  },
  type: 'outlabeledPie',
  data: {...(() => {
    let a = {
      labels: [],
      datasets: [{
        backgroundColor: [],
        data: [],
        borderColor: 'rgba(22,22,22,0.3)',
        borderWidth: 2
      }]
    };
`
994400  1.91s Reloading;
001799  3.17s Loading Resource - AssetLibrary;
179900  5.84s Preloading 53258 textures;
0D9900  2.92s Texture loading;
009911  1.62s Texture mipmap and upload;
009926  5.11s Posting bake events;
009930 38.20s Setting up dynamic models;
00993A 38.28s Loading Resource - ModelManager;
009299 39.30s Rendering Setup;
300099  1.32s File;
440099  2.72s Recipe;
4F0099  4.29s XML Recipes;
590099  5.55s InterModComms;
991100  1.29s Ender IO;
99007D 12.01s [VintageFix]: Texture search 69847 sprites;
990073  5.98s Preloaded 33668 sprites
`
    .split(';')
      .map(l => l.match(/(\w{6}) *(\d*\.\d*) ?s (.*)/s))
      .forEach(([, col, time, name]) => {
        a.labels.push([
          name.length > 15 ? name.split(/(?%3C=.{11})\s(?=\S{6})/).join('\n') : name
          , ' ', time, 's'
        ].join(''));
        a.datasets[0].data.push(parseFloat(time));
        a.datasets[0].backgroundColor.push([String.fromCharCode(35), col].join(''))
      })
      ; return a
  })()}
}"/>
</p>
