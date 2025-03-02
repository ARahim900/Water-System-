import React, { useState, useEffect } from 'react';
import { LineChart, Line, BarChart, Bar, ComposedChart, XAxis, YAxis, CartesianGrid, Tooltip, Legend, ResponsiveContainer, Cell, PieChart, Pie, Sector, RadarChart, Radar, PolarGrid, PolarAngleAxis, PolarRadiusAxis, AreaChart, Area, Label } from 'recharts';
import _ from 'lodash';

const MuscatBayDashboard = () => {
  // State
  const [activeView, setActiveView] = useState('executive');
  const [activeZone, setActiveZone] = useState('all');
  const [activeMonth, setActiveMonth] = useState('Total');
  const [comparisonMode, setComparisonMode] = useState('levels'); // 'levels', 'zones', 'months'
  const [selectedMetric, setSelectedMetric] = useState('consumption'); // 'consumption', 'loss', 'efficiency'
  const [focusLevel, setFocusLevel] = useState('all'); // 'all', 'L1', 'L2', 'L3'
  
  const months = ['Jan-24', 'Feb-24', 'Mar-24', 'Apr-24', 'May-24', 'Jun-24', 
                 'Jul-24', 'Aug-24', 'Sep-24', 'Oct-24', 'Nov-24', 'Dec-24', 'Total'];
                 
  // Dashboard color theme
  const theme = {
    primary: '#0ea5e9',
    secondary: '#6366f1',
    accent: '#8b5cf6',
    success: '#10b981',
    warning: '#f59e0b',
    danger: '#ef4444',
    info: '#3b82f6',
    neutral: '#6b7280',
    light: '#f3f4f6',
    dark: '#1f2937',
    background: '#f8fafc',
  };
  
  // Chart colors
  const CHART_COLORS = [
    theme.primary, 
    theme.secondary, 
    theme.accent, 
    theme.info, 
    theme.success, 
    theme.warning, 
    theme.danger
  ];
  
  // Simulate data for Muscat Bay water system based on the document
  // Main data structure
  const waterData = {
    mainBulk: {
      label: "Main Bulk (NAMA)",
      total: 404687,
      monthly: {
        'Jan-24': 32803, 'Feb-24': 27996, 'Mar-24': 23860, 'Apr-24': 31869, 
        'May-24': 30737, 'Jun-24': 41953, 'Jul-24': 35166, 'Aug-24': 35420, 
        'Sep-24': 41341, 'Oct-24': 31519, 'Nov-24': 35290, 'Dec-24': 36733
      }
    },
    zoneBulkAndDirect: {
      total: 369221,
      zones: {
        'FM': { 
          bulkMeters: { total: 20646 },
          directMeters: { total: 0 }
        },
        '3A': { 
          bulkMeters: { total: 30198 },
          directMeters: { total: 0 }
        },
        '3B': { 
          bulkMeters: { total: 34837 },
          directMeters: { total: 0 }
        },
        '5': { 
          bulkMeters: { total: 45541 },
          directMeters: { total: 0 }
        },
        '8': { 
          bulkMeters: { total: 37110 },
          directMeters: { total: 0 }
        },
        'VS': { 
          bulkMeters: { total: 1118 },
          directMeters: { total: 0 }
        },
        'Direct': { 
          directMeters: { total: 199771 },
          bulkMeters: { total: 0 }
        }
      },
      // Assuming monthly breakdowns - simplified for demo
      monthly: {
        'Jan-24': 30000, 'Feb-24': 26000, 'Mar-24': 22000, 'Apr-24': 30000, 
        'May-24': 28000, 'Jun-24': 38000, 'Jul-24': 32000, 'Aug-24': 32000, 
        'Sep-24': 38000, 'Oct-24': 30000, 'Nov-24': 32000, 'Dec-24': 31221
      }
    },
    individualMeters: {
      total: 297515,
      zones: {
        'FM': { total: 19806 },
        '3A': { total: 16139 },
        '3B': { total: 19434 },
        '5': { total: 15609 },
        '8': { total: 26409 },
        'VS': { total: 839 },
        'Hotel': { total: 167796 },
        'Sale Center': { total: 502 }
      },
      // Assuming monthly breakdowns - simplified for demo
      monthly: {
        'Jan-24': 24500, 'Feb-24': 21000, 'Mar-24': 18000, 'Apr-24': 24500, 
        'May-24': 22500, 'Jun-24': 31000, 'Jul-24': 26000, 'Aug-24': 26000, 
        'Sep-24': 31000, 'Oct-24': 24500, 'Nov-24': 25500, 'Dec-24': 23015
      }
    },
    irrigationMeters: {
      total: 31863,
      meters: [
        { name: "Irrigation Tank (Z01_FM)", zone: "Zone_01_(FM)", total: 1396 },
        { name: "Irrigation Tank 01 (Inlet)", zone: "Zone_Technical", total: 0 },
        { name: "Irrigation Tank 02 (Z03)", zone: "Zone_03_(B)", total: 494 },
        { name: "Irrigation Tank 03 (Z05)", zone: "Zone_05", total: 4710 },
        { name: "Irrigation Tank 04 – (Z08)", zone: "Others", total: 6664 },
        { name: "Irrigation–Controller DOWN Rd(01)_IRR", zone: "(Main Bulk)", total: 9447 },
        { name: "Irrigation–Controller UP Rd(01)_IRR", zone: "(Main Bulk)", total: 8705 },
        { name: "Irrigation Tank – VS", zone: "Zone_VS", total: 447 }
      ],
      // Simplified monthly breakdown
      monthly: {
        'Jan-24': 2500, 'Feb-24': 2200, 'Mar-24': 1900, 'Apr-24': 2600, 
        'May-24': 2700, 'Jun-24': 3800, 'Jul-24': 3500, 'Aug-24': 3400, 
        'Sep-24': 3500, 'Oct-24': 2600, 'Nov-24': 2300, 'Dec-24': 863
      }
    }
  };
  
  // Calculate loss data
  const lossData = {
    mainToZone: {
      total: waterData.mainBulk.total - waterData.zoneBulkAndDirect.total,
      percentage: ((waterData.mainBulk.total - waterData.zoneBulkAndDirect.total) / waterData.mainBulk.total * 100).toFixed(2),
      monthly: {}
    },
    zoneToIndividual: {
      total: waterData.zoneBulkAndDirect.total - waterData.individualMeters.total,
      percentage: ((waterData.zoneBulkAndDirect.total - waterData.individualMeters.total) / waterData.zoneBulkAndDirect.total * 100).toFixed(2),
      monthly: {}
    },
    mainToIndividual: {
      total: waterData.mainBulk.total - waterData.individualMeters.total,
      percentage: ((waterData.mainBulk.total - waterData.individualMeters.total) / waterData.mainBulk.total * 100).toFixed(2),
      monthly: {}
    },
    zoneBreakdown: {
      'FM': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['FM'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['FM'].total,
        difference: waterData.zoneBulkAndDirect.zones['FM'].bulkMeters.total - waterData.individualMeters.zones['FM'].total
      },
      '3A': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['3A'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['3A'].total,
        difference: waterData.zoneBulkAndDirect.zones['3A'].bulkMeters.total - waterData.individualMeters.zones['3A'].total
      },
      '3B': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['3B'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['3B'].total,
        difference: waterData.zoneBulkAndDirect.zones['3B'].bulkMeters.total - waterData.individualMeters.zones['3B'].total
      },
      '5': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['5'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['5'].total,
        difference: waterData.zoneBulkAndDirect.zones['5'].bulkMeters.total - waterData.individualMeters.zones['5'].total
      },
      '8': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['8'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['8'].total,
        difference: waterData.zoneBulkAndDirect.zones['8'].bulkMeters.total - waterData.individualMeters.zones['8'].total
      },
      'VS': { 
        bulkTotal: waterData.zoneBulkAndDirect.zones['VS'].bulkMeters.total,
        individualTotal: waterData.individualMeters.zones['VS'].total,
        difference: waterData.zoneBulkAndDirect.zones['VS'].bulkMeters.total - waterData.individualMeters.zones['VS'].total
      }
    }
  };
  
  // Fill monthly loss data
  Object.keys(waterData.mainBulk.monthly).forEach(month => {
    lossData.mainToZone.monthly[month] = waterData.mainBulk.monthly[month] - waterData.zoneBulkAndDirect.monthly[month];
    lossData.zoneToIndividual.monthly[month] = waterData.zoneBulkAndDirect.monthly[month] - waterData.individualMeters.monthly[month];
    lossData.mainToIndividual.monthly[month] = waterData.mainBulk.monthly[month] - waterData.individualMeters.monthly[month];
  });
  
  // Key Performance Indicators
  const kpiData = {
    totalAccounted: waterData.individualMeters.total,
    totalUnaccounted: waterData.mainBulk.total - waterData.individualMeters.total,
    accountedPercentage: ((waterData.individualMeters.total / waterData.mainBulk.total) * 100).toFixed(1),
    lossPercentage: (((waterData.mainBulk.total - waterData.individualMeters.total) / waterData.mainBulk.total) * 100).toFixed(1),
    worstZone: 'Zone 5',
    worstZoneLoss: 29932,
    bestZone: 'FM',
    bestZoneLoss: 840,
    alertZones: ['Zone 5', 'Zone 3B', 'Zone 3A'],
  };
  
  // Trending data - simulate declining losses after interventions
  const trendData = months.slice(0, -1).map((month, index) => {
    let lossPercentage = 26 - (index * 0.5); // Gradually declining from 26%
    if (index === 5) lossPercentage -= 2; // Simulate intervention effect
    if (index === 9) lossPercentage -= 1.5; // Simulate another intervention
    
    return {
      month,
      lossPercentage: lossPercentage,
      target: 15, // Target loss percentage
      industry: 18 // Industry average
    };
  });
  
  // Action plan data
  const actionPlanData = [
    { 
      id: 1, 
      title: "Data Review with Tadoom", 
      description: "Analyze smart meter data discrepancies", 
      status: "In Progress", 
      progress: 65, 
      priority: "High", 
      deadline: "2025-03-15" 
    },
    { 
      id: 2, 
      title: "System Check in Zone 5", 
      description: "Full assessment of meter performance", 
      status: "Scheduled", 
      progress: 20, 
      priority: "Critical", 
      deadline: "2025-03-10" 
    },
    { 
      id: 3, 
      title: "Technical Support Protocol", 
      description: "Establish dedicated support for urgent issues", 
      status: "Completed", 
      progress: 100, 
      priority: "Medium", 
      deadline: "2025-02-28" 
    },
    { 
      id: 4, 
      title: "Monthly Progress Review", 
      description: "Regular meetings with Tadoom team", 
      status: "Ongoing", 
      progress: 40, 
      priority: "Medium", 
      deadline: "2025-04-30" 
    },
    { 
      id: 5, 
      title: "Irrigation Efficiency in Zone 5", 
      description: "Investigate high irrigation water usage", 
      status: "Pending", 
      progress: 0, 
      priority: "High", 
      deadline: "2025-04-15" 
    }
  ];
  
  // Helper function for formatting numbers
  const formatNumber = (num) => {
    return num.toLocaleString();
  };
  
  // Prepare chart data
  const prepareMonthlyTrendData = () => {
    return months.slice(0, -1).map(month => ({
      name: month,
      'Main Bulk': waterData.mainBulk.monthly[month],
      'Zone Bulk & Direct': waterData.zoneBulkAndDirect.monthly[month],
      'Individual Meters': waterData.individualMeters.monthly[month],
      'Irrigation': waterData.irrigationMeters.monthly[month]
    }));
  };
  
  const prepareLossComparisonData = () => {
    return [
      { 
        name: 'Main → Zone (L1-L2)', 
        value: lossData.mainToZone.total,
        percentage: parseFloat(lossData.mainToZone.percentage)
      },
      { 
        name: 'Zone → Individual (L2-L3)', 
        value: lossData.zoneToIndividual.total,
        percentage: parseFloat(lossData.zoneToIndividual.percentage)
      },
      { 
        name: 'Main → Individual (L1-L3)', 
        value: lossData.mainToIndividual.total,
        percentage: parseFloat(lossData.mainToIndividual.percentage)
      }
    ];
  };
  
  const prepareZoneLossData = () => {
    return Object.entries(lossData.zoneBreakdown).map(([zone, data]) => ({
      name: `Zone ${zone}`,
      bulk: data.bulkTotal,
      individual: data.individualTotal,
      loss: data.difference,
      lossPercentage: data.bulkTotal ? (data.difference / data.bulkTotal * 100).toFixed(1) : 0
    })).sort((a, b) => b.loss - a.loss);
  };
  
  const prepareWaterDistributionData = () => {
    return [
      { name: 'Individual Consumption', value: waterData.individualMeters.total },
      { name: 'Unaccounted Water', value: lossData.mainToIndividual.total }
    ];
  };
  
  const prepareZoneDistributionData = () => {
    return Object.entries(waterData.individualMeters.zones).map(([zone, data]) => ({
      name: zone === 'FM' ? 'Zone FM' : 
            zone === 'VS' ? 'Village Square' : 
            zone === 'Sale Center' ? 'Sale Center' : `Zone ${zone}`,
      value: data.total
    }));
  };
  
  const prepareMonthlyLossData = () => {
    return months.slice(0, -1).map(month => ({
      month,
      'Main to Zone': lossData.mainToZone.monthly[month],
      'Zone to Individual': lossData.zoneToIndividual.monthly[month],
      'Total Loss': lossData.mainToIndividual.monthly[month],
    }));
  };
  
  const calcLossForMonth = (month) => {
    const main = waterData.mainBulk.monthly[month] || 0;
    const individual = waterData.individualMeters.monthly[month] || 0;
    return {
      volume: main - individual,
      percentage: main ? ((main - individual) / main * 100).toFixed(1) : 0
    };
  };
  
  // Get color based on efficiency (red->yellow->green scale)
  const getEfficiencyColor = (percentage) => {
    if (percentage >= 90) return theme.success;
    if (percentage >= 80) return theme.warning;
    return theme.danger;
  };
  
  // Get color based on loss percentage (green->yellow->red scale)
  const getLossColor = (percentage) => {
    if (percentage <= 10) return theme.success;
    if (percentage <= 20) return theme.warning;
    return theme.danger;
  };
  
  // Status card components
  const StatusCard = ({ title, value, subtitle, trend, icon, color, onClick }) => (
    <div 
      className="bg-white rounded-lg shadow-md p-4 cursor-pointer hover:shadow-lg transition-shadow"
      onClick={onClick}
    >
      <div className="flex items-start justify-between">
        <div>
          <p className="text-sm text-gray-500 mb-1">{title}</p>
          <h3 className="text-xl font-bold" style={{ color }}>{value}</h3>
          <p className="text-xs text-gray-600 mt-1">{subtitle}</p>
        </div>
        <div className={`rounded-full p-2`} style={{ backgroundColor: `${color}25` }}>
          <span className="text-xl" style={{ color }}>{icon}</span>
        </div>
      </div>
      {trend && (
        <div className="mt-2 text-xs font-medium" style={{ color: trend.direction === 'up' ? theme.danger : theme.success }}>
          {trend.direction === 'up' ? '↑' : '↓'} {trend.value}% from previous month
        </div>
      )}
    </div>
  );
  
  // Active shape for pie chart
  const renderActiveShape = (props) => {
    const RADIAN = Math.PI / 180;
    const { cx, cy, midAngle, innerRadius, outerRadius, startAngle, endAngle,
      fill, payload, percent, value } = props;
    const sin = Math.sin(-RADIAN * midAngle);
    const cos = Math.cos(-RADIAN * midAngle);
    const sx = cx + (outerRadius + 10) * cos;
    const sy = cy + (outerRadius + 10) * sin;
    const mx = cx + (outerRadius + 30) * cos;
    const my = cy + (outerRadius + 30) * sin;
    const ex = mx + (cos >= 0 ? 1 : -1) * 22;
    const ey = my;
    const textAnchor = cos >= 0 ? 'start' : 'end';
  
    return (
      <g>
        <text x={cx} y={cy} dy={8} textAnchor="middle" fill={fill} className="text-sm font-medium">
          {payload.name}
        </text>
        <Sector
          cx={cx}
          cy={cy}
          innerRadius={innerRadius}
          outerRadius={outerRadius}
          startAngle={startAngle}
          endAngle={endAngle}
          fill={fill}
        />
        <Sector
          cx={cx}
          cy={cy}
          startAngle={startAngle}
          endAngle={endAngle}
          innerRadius={outerRadius + 6}
          outerRadius={outerRadius + 10}
          fill={fill}
        />
        <path d={`M${sx},${sy}L${mx},${my}L${ex},${ey}`} stroke={fill} fill="none" />
        <circle cx={ex} cy={ey} r={2} fill={fill} stroke="none" />
        <text x={ex + (cos >= 0 ? 1 : -1) * 12} y={ey} textAnchor={textAnchor} fill="#333" className="text-xs">
          {`${formatNumber(value)} m³`}
        </text>
        <text x={ex + (cos >= 0 ? 1 : -1) * 12} y={ey} dy={18} textAnchor={textAnchor} fill="#999" className="text-xs">
          {`(${(percent * 100).toFixed(1)}%)`}
        </text>
      </g>
    );
  };

  // Progress bar component
  const ProgressBar = ({ value, color }) => (
    <div className="w-full bg-gray-200 rounded-full h-2">
      <div 
        className="rounded-full h-2" 
        style={{ 
          width: `${value}%`, 
          backgroundColor: color
        }}
      ></div>
    </div>
  );
  
  // Tab button component
  const TabButton = ({ label, active, onClick, icon }) => (
    <button
      className={`px-4 py-2 rounded-md flex items-center gap-2 ${
        active 
          ? 'bg-blue-600 text-white shadow-md' 
          : 'bg-white text-gray-700 hover:bg-gray-100'
      }`}
      onClick={onClick}
    >
      {icon && <span>{icon}</span>}
      <span>{label}</span>
    </button>
  );
  
  // Quick filter component
  const QuickFilter = ({ label, options, value, onChange }) => (
    <div className="flex items-center gap-2">
      <span className="text-xs text-gray-500">{label}:</span>
      <select 
        className="text-sm border border-gray-300 rounded px-2 py-1"
        value={value}
        onChange={(e) => onChange(e.target.value)}
      >
        {options.map(option => (
          <option key={option.value} value={option.value}>{option.label}</option>
        ))}
      </select>
    </div>
  );
  
  // Card with title component
  const Card = ({ title, subtitle, children, className = "", action }) => (
    <div className={`bg-white rounded-lg shadow-md overflow-hidden ${className}`}>
      <div className="p-4 border-b border-gray-200 flex justify-between items-center">
        <div>
          <h3 className="font-semibold text-gray-800">{title}</h3>
          {subtitle && <p className="text-xs text-gray-500 mt-1">{subtitle}</p>}
        </div>
        {action && <div>{action}</div>}
      </div>
      <div className="p-4">
        {children}
      </div>
    </div>
  );
  
  // Executive Summary View
  const ExecutiveView = () => (
    <div className="space-y-6">
      {/* KPI Header */}
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
        <StatusCard 
          title="Total Water Consumption" 
          value={`${formatNumber(waterData.mainBulk.total)} m³`}
          subtitle="Main Bulk Meter Reading" 
          icon="🌊" 
          color={theme.primary}
          onClick={() => {
            setActiveView('consumption');
            setFocusLevel('L1');
          }}
        />
        <StatusCard 
          title="Accounted Water" 
          value={`${kpiData.accountedPercentage}%`}
          subtitle={`${formatNumber(kpiData.totalAccounted)} m³ measured at end points`}
          icon="✓" 
          color={getEfficiencyColor(parseFloat(kpiData.accountedPercentage))}
          onClick={() => {
            setActiveView('consumption');
            setFocusLevel('L3');
          }}
        />
        <StatusCard 
          title="Water Loss" 
          value={`${formatNumber(kpiData.totalUnaccounted)} m³`}
          subtitle={`${kpiData.lossPercentage}% of total supply`}
          icon="💧" 
          color={getLossColor(parseFloat(kpiData.lossPercentage))}
          trend={{ direction: 'down', value: '2.3' }}
          onClick={() => setActiveView('losses')}
        />
        <StatusCard 
          title="Critical Zone" 
          value={kpiData.worstZone}
          subtitle={`${formatNumber(kpiData.worstZoneLoss)} m³ unaccounted water`}
          icon="⚠️" 
          color={theme.danger}
          onClick={() => {
            setActiveView('zones');
            setActiveZone(kpiData.worstZone.replace('Zone ', ''));
          }}
        />
      </div>
      
      {/* Main Overview Charts */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card 
          title="Water Consumption Trend" 
          subtitle="Monthly consumption by level"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <ComposedChart data={prepareMonthlyTrendData()}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="name" />
                <YAxis>
                  <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
                </YAxis>
                <Tooltip formatter={(value) => formatNumber(value)} />
                <Legend />
                <Area type="monotone" dataKey="Main Bulk" fill={`${theme.primary}40`} stroke={theme.primary} />
                <Area type="monotone" dataKey="Zone Bulk & Direct" fill={`${theme.secondary}40`} stroke={theme.secondary} />
                <Line type="monotone" dataKey="Individual Meters" stroke={theme.success} strokeWidth={2} dot={{ strokeWidth: 2 }} />
                <Bar dataKey="Irrigation" barSize={20} fill={theme.info} />
              </ComposedChart>
            </ResponsiveContainer>
          </div>
        </Card>
        
        <Card 
          title="Water Loss Analysis" 
          subtitle="Breakdown by classification level"
        >
          <div className="h-80 grid grid-cols-1 lg:grid-cols-2 gap-4">
            <div>
              <ResponsiveContainer width="100%" height="100%">
                <PieChart>
                  <Pie
                    activeIndex={0}
                    activeShape={renderActiveShape}
                    data={prepareWaterDistributionData()}
                    innerRadius={60}
                    outerRadius={80}
                    dataKey="value"
                    nameKey="name"
                  >
                    {prepareWaterDistributionData().map((entry, index) => (
                      <Cell key={`cell-${index}`} fill={index === 0 ? theme.success : theme.danger} />
                    ))}
                  </Pie>
                  <Tooltip formatter={(value) => formatNumber(value)} />
                </PieChart>
              </ResponsiveContainer>
            </div>
            <div className="flex flex-col justify-center space-y-4">
              <div>
                <div className="flex justify-between mb-1">
                  <span className="text-sm text-gray-600">L1 → L2 Loss</span>
                  <span className="text-sm font-medium">{lossData.mainToZone.percentage}%</span>
                </div>
                <ProgressBar value={lossData.mainToZone.percentage} color={getLossColor(lossData.mainToZone.percentage)} />
              </div>
              <div>
                <div className="flex justify-between mb-1">
                  <span className="text-sm text-gray-600">L2 → L3 Loss</span>
                  <span className="text-sm font-medium">{lossData.zoneToIndividual.percentage}%</span>
                </div>
                <ProgressBar value={lossData.zoneToIndividual.percentage} color={getLossColor(lossData.zoneToIndividual.percentage)} />
              </div>
              <div>
                <div className="flex justify-between mb-1">
                  <span className="text-sm text-gray-600">Total System Loss</span>
                  <span className="text-sm font-medium">{lossData.mainToIndividual.percentage}%</span>
                </div>
                <ProgressBar value={lossData.mainToIndividual.percentage} color={getLossColor(lossData.mainToIndividual.percentage)} />
              </div>
            </div>
          </div>
        </Card>
      </div>
      
      {/* Zone Comparison & Action Plan */}
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card 
          title="Zone Efficiency Comparison" 
          subtitle="Analysis of water loss by zone"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <BarChart data={prepareZoneLossData()}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="name" />
                <YAxis yAxisId="left" orientation="left">
                  <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
                </YAxis>
                <YAxis yAxisId="right" orientation="right" tickFormatter={(value) => `${value}%`}>
                  <Label value="Loss %" angle={90} position="insideRight" style={{ textAnchor: 'middle' }} />
                </YAxis>
                <Tooltip formatter={(value, name) => [
                  formatNumber(value), 
                  name === 'lossPercentage' ? 'Loss %' : name.charAt(0).toUpperCase() + name.slice(1)
                ]} />
                <Legend />
                <Bar yAxisId="left" dataKey="bulk" name="Zone Bulk" fill={theme.secondary} stackId="a" />
                <Bar yAxisId="left" dataKey="individual" name="Individual" fill={theme.success} stackId="a" />
                <Line yAxisId="right" type="monotone" dataKey="lossPercentage" name="Loss %" stroke={theme.danger} strokeWidth={2} />
              </BarChart>
            </ResponsiveContainer>
          </div>
        </Card>
        
        <Card 
          title="Action Plan Status" 
          subtitle="Current improvement initiatives"
          action={
            <button 
              className="text-sm text-blue-600 hover:text-blue-800"
              onClick={() => setActiveView('actions')}
            >
              View All
            </button>
          }
        >
          <div className="space-y-4 h-80 overflow-y-auto">
            {actionPlanData.map(action => (
              <div key={action.id} className="border-b border-gray-100 pb-3 last:border-0">
                <div className="flex justify-between mb-1">
                  <h4 className="font-medium text-gray-800">{action.title}</h4>
                  <span 
                    className={`text-xs px-2 py-1 rounded-full ${
                      action.status === 'Completed' ? 'bg-green-100 text-green-800' :
                      action.status === 'In Progress' ? 'bg-blue-100 text-blue-800' :
                      action.status === 'Pending' ? 'bg-gray-100 text-gray-800' :
                      'bg-yellow-100 text-yellow-800'
                    }`}
                  >
                    {action.status}
                  </span>
                </div>
                <p className="text-sm text-gray-600 mb-2">{action.description}</p>
                <div className="flex justify-between text-xs text-gray-500 mb-1">
                  <span>Progress: {action.progress}%</span>
                  <span>Priority: 
                    <span 
                      className={`ml-1 font-medium ${
                        action.priority === 'Critical' ? 'text-red-600' :
                        action.priority === 'High' ? 'text-orange-600' :
                        'text-blue-600'
                      }`}
                    >
                      {action.priority}
                    </span>
                  </span>
                </div>
                <ProgressBar 
                  value={action.progress} 
                  color={
                    action.priority === 'Critical' ? theme.danger :
                    action.priority === 'High' ? theme.warning :
                    theme.info
                  } 
                />
              </div>
            ))}
          </div>
        </Card>
      </div>
      
      {/* Trending Comparison */}
      <Card 
        title="Loss Reduction Progress" 
        subtitle="Tracking improvement over time against targets"
      >
        <div className="h-64">
          <ResponsiveContainer width="100%" height="100%">
            <LineChart data={trendData}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="month" />
              <YAxis tickFormatter={(value) => `${value}%`}>
                <Label value="Loss Percentage" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <Tooltip formatter={(value) => `${value}%`} />
              <Legend />
              <Line type="monotone" dataKey="lossPercentage" name="Actual Loss %" stroke={theme.primary} strokeWidth={2} activeDot={{ r: 8 }} />
              <Line type="monotone" dataKey="target" name="Target" stroke={theme.success} strokeDasharray="5 5" />
              <Line type="monotone" dataKey="industry" name="Industry Avg" stroke={theme.neutral} strokeDasharray="3 3" />
            </LineChart>
          </ResponsiveContainer>
        </div>
      </Card>
    </div>
  );
  
  // Consumption View
  const ConsumptionView = () => (
    <div className="space-y-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <StatusCard 
          title="L1: Main Bulk" 
          value={`${formatNumber(waterData.mainBulk.total)} m³`}
          subtitle="Total supply entering the system" 
          icon="🏢" 
          color={theme.primary}
          onClick={() => setFocusLevel('L1')}
        />
        <StatusCard 
          title="L2: Zone & Direct" 
          value={`${formatNumber(waterData.zoneBulkAndDirect.total)} m³`}
          subtitle="Combined zone bulk and direct connections" 
          icon="🏘️" 
          color={theme.secondary}
          onClick={() => setFocusLevel('L2')}
        />
        <StatusCard 
          title="L3: End Users" 
          value={`${formatNumber(waterData.individualMeters.total)} m³`}
          subtitle="Individual consumption points" 
          icon="👥" 
          color={theme.success}
          onClick={() => setFocusLevel('L3')}
        />
      </div>
      
      <Card 
        title="Consumption Breakdown" 
        subtitle={`Water consumption across all measurement levels (${activeMonth})`}
        action={
          <div className="flex gap-2">
            <select 
              className="text-sm border border-gray-300 rounded px-2 py-1"
              value={activeMonth}
              onChange={(e) => setActiveMonth(e.target.value)}
            >
              {months.map(month => (
                <option key={month} value={month}>{month}</option>
              ))}
            </select>
            <select 
              className="text-sm border border-gray-300 rounded px-2 py-1"
              value={focusLevel}
              onChange={(e) => setFocusLevel(e.target.value)}
            >
              <option value="all">All Levels</option>
              <option value="L1">L1 (Main Bulk)</option>
              <option value="L2">L2 (Zone & Direct)</option>
              <option value="L3">L3 (End Users)</option>
            </select>
          </div>
        }
      >
        <div className="h-96">
          <ResponsiveContainer width="100%" height="100%">
            <ComposedChart data={prepareMonthlyTrendData()}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis>
                <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <Tooltip formatter={(value) => formatNumber(value)} />
              <Legend />
              {(focusLevel === 'all' || focusLevel === 'L1') && 
                <Area type="monotone" dataKey="Main Bulk" fill={`${theme.primary}40`} stroke={theme.primary} />
              }
              {(focusLevel === 'all' || focusLevel === 'L2') && 
                <Area type="monotone" dataKey="Zone Bulk & Direct" fill={`${theme.secondary}40`} stroke={theme.secondary} />
              }
              {(focusLevel === 'all' || focusLevel === 'L3') && 
                <Area type="monotone" dataKey="Individual Meters" fill={`${theme.success}40`} stroke={theme.success} />
              }
              {(focusLevel === 'all' || focusLevel === 'L3') && 
                <Line type="monotone" dataKey="Irrigation" stroke={theme.info} strokeWidth={2} />
              }
            </ComposedChart>
          </ResponsiveContainer>
        </div>
      </Card>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card 
          title="L3 Consumption by Zone" 
          subtitle="Distribution of end-user consumption"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <BarChart data={prepareZoneDistributionData()}>
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="name" />
                <YAxis>
                  <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
                </YAxis>
                <Tooltip formatter={(value) => formatNumber(value)} />
                <Bar dataKey="value" name="Consumption">
                  {prepareZoneDistributionData().map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={CHART_COLORS[index % CHART_COLORS.length]} />
                  ))}
                </Bar>
              </BarChart>
            </ResponsiveContainer>
          </div>
        </Card>
        
        <Card 
          title="L3 Share by Category" 
          subtitle="End-user consumption by category"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <PieChart>
                <Pie
                  data={[
                    { name: 'Residential (Villa)', value: 98000 },
                    { name: 'Residential (Apart)', value: 162000 },
                    { name: 'Hotel', value: 17796 },
                    { name: 'Retail', value: 7500 },
                    { name: 'Common Areas', value: 12219 }
                  ]}
                  innerRadius={70}
                  outerRadius={90}
                  dataKey="value"
                  nameKey="name"
                  label
                >
                  {prepareZoneDistributionData().map((entry, index) => (
                    <Cell key={`cell-${index}`} fill={CHART_COLORS[index % CHART_COLORS.length]} />
                  ))}
                </Pie>
                <Tooltip formatter={(value) => formatNumber(value)} />
                <Legend />
              </PieChart>
            </ResponsiveContainer>
          </div>
        </Card>
      </div>
      
      <Card 
        title="Irrigation Water Usage" 
        subtitle="Breakdown of irrigation consumption by location"
      >
        <div className="h-80">
          <ResponsiveContainer width="100%" height="100%">
            <BarChart 
              data={waterData.irrigationMeters.meters.filter(m => m.total > 0)}
              margin={{ top: 20, right: 30, left: 20, bottom: 5 }}
            >
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis>
                <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <Tooltip formatter={(value) => formatNumber(value)} />
              <Bar dataKey="total" name="Consumption" fill={theme.info}>
                {waterData.irrigationMeters.meters.filter(m => m.total > 0).map((entry, index) => (
                  <Cell key={`cell-${index}`} fill={CHART_COLORS[(index + 2) % CHART_COLORS.length]} />
                ))}
              </Bar>
            </BarChart>
          </ResponsiveContainer>
        </div>
      </Card>
    </div>
  );
  
  // Loss Analysis View
  const LossView = () => (
    <div className="space-y-6">
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <StatusCard 
          title="Main Line Losses" 
          value={`${formatNumber(lossData.mainToZone.total)} m³`}
          subtitle={`${lossData.mainToZone.percentage}% of Main Bulk`} 
          icon="🔌" 
          color={getLossColor(lossData.mainToZone.percentage)}
        />
        <StatusCard 
          title="Zone Distribution Losses" 
          value={`${formatNumber(lossData.zoneToIndividual.total)} m³`}
          subtitle={`${lossData.zoneToIndividual.percentage}% of Zone Bulk`} 
          icon="🔄" 
          color={getLossColor(lossData.zoneToIndividual.percentage)}
        />
        <StatusCard 
          title="Total System Losses" 
          value={`${formatNumber(lossData.mainToIndividual.total)} m³`}
          subtitle={`${lossData.mainToIndividual.percentage}% of Main Bulk`} 
          icon="⚠️" 
          color={getLossColor(lossData.mainToIndividual.percentage)}
        />
      </div>
      
      <Card 
        title="Loss Breakdown by Level" 
        subtitle="Comparing losses between system levels"
      >
        <div className="h-80">
          <ResponsiveContainer width="100%" height="100%">
            <BarChart data={prepareLossComparisonData()}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="name" />
              <YAxis yAxisId="left">
                <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <YAxis yAxisId="right" orientation="right" tickFormatter={(value) => `${value}%`}>
                <Label value="Percentage" angle={90} position="insideRight" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <Tooltip formatter={(value, name) => [
                name === 'percentage' ? `${value}%` : formatNumber(value),
                name === 'percentage' ? 'Loss Percentage' : 'Volume'
              ]} />
              <Legend />
              <Bar yAxisId="left" dataKey="value" name="Volume" fill={theme.danger} />
              <Line yAxisId="right" type="monotone" dataKey="percentage" name="Percentage" stroke={theme.primary} strokeWidth={2} />
            </BarChart>
          </ResponsiveContainer>
        </div>
      </Card>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card 
          title="Zone Loss Analysis" 
          subtitle="Loss by zone (sorted by volume)"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <ComposedChart 
                data={prepareZoneLossData()}
                margin={{ top: 20, right: 30, left: 20, bottom: 5 }}
              >
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis dataKey="name" />
                <YAxis>
                  <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
                </YAxis>
                <Tooltip formatter={(value) => formatNumber(value)} />
                <Legend />
                <Bar dataKey="loss" name="Loss Volume" fill={theme.danger} />
              </ComposedChart>
            </ResponsiveContainer>
          </div>
        </Card>
        
        <Card 
          title="Zone Efficiency Rating" 
          subtitle="Percentage of water accounted for by zone"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <RadarChart outerRadius={90} data={prepareZoneLossData().map(zone => ({
                zone: zone.name,
                efficiency: 100 - parseFloat(zone.lossPercentage),
                lossPercentage: parseFloat(zone.lossPercentage)
              }))}>
                <PolarGrid />
                <PolarAngleAxis dataKey="zone" />
                <PolarRadiusAxis angle={30} domain={[0, 100]} />
                <Radar name="Efficiency %" dataKey="efficiency" stroke={theme.success} fill={theme.success} fillOpacity={0.6} />
                <Radar name="Loss %" dataKey="lossPercentage" stroke={theme.danger} fill={theme.danger} fillOpacity={0.6} />
                <Tooltip formatter={(value) => `${value.toFixed(1)}%`} />
                <Legend />
              </RadarChart>
            </ResponsiveContainer>
          </div>
        </Card>
      </div>
      
      <Card 
        title="Monthly Loss Trend" 
        subtitle="Tracking water losses over time"
      >
        <div className="h-80">
          <ResponsiveContainer width="100%" height="100%">
            <ComposedChart data={prepareMonthlyLossData()}>
              <CartesianGrid strokeDasharray="3 3" />
              <XAxis dataKey="month" />
              <YAxis>
                <Label value="Volume (m³)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
              </YAxis>
              <Tooltip formatter={(value) => formatNumber(value)} />
              <Legend />
              <Area type="monotone" dataKey="Total Loss" fill={`${theme.danger}40`} stroke={theme.danger} />
              <Bar dataKey="Main to Zone" name="Main to Zone Loss" fill={theme.secondary} />
              <Bar dataKey="Zone to Individual" name="Zone to Individual Loss" fill={theme.warning} />
            </ComposedChart>
          </ResponsiveContainer>
        </div>
      </Card>
    </div>
  );
  
  // Zone Analysis View
  const ZoneView = () => {
    const zoneData = prepareZoneLossData().find(z => z.name === `Zone ${activeZone}`) || prepareZoneLossData()[0];
    
    return (
      <div className="space-y-6">
        <div className="flex flex-wrap items-center justify-between gap-4 bg-white p-4 rounded-lg shadow-md">
          <div>
            <h2 className="text-xl font-bold text-gray-800">Zone {activeZone} Analysis</h2>
            <p className="text-sm text-gray-600">Detailed view of consumption and losses</p>
          </div>
          <select 
            className="border border-gray-300 rounded-md px-3 py-1.5"
            value={activeZone}
            onChange={(e) => setActiveZone(e.target.value)}
          >
            {prepareZoneLossData().map(zone => (
              <option key={zone.name} value={zone.name.replace('Zone ', '')}>
                {zone.name}
              </option>
            ))}
          </select>
        </div>
        
        <div className="grid grid-cols-1 md:grid-cols-4 gap-4">
          <StatusCard 
            title="Bulk Meter Reading" 
            value={`${formatNumber(zoneData.bulk)} m³`}
            subtitle="Total entering the zone" 
            icon="📊" 
            color={theme.primary}
          />
          <StatusCard 
            title="Individual Consumption" 
            value={`${formatNumber(zoneData.individual)} m³`}
            subtitle="Measured at end points" 
            icon="👥" 
            color={theme.success}
          />
          <StatusCard 
            title="Unaccounted Water" 
            value={`${formatNumber(zoneData.loss)} m³`}
            subtitle={`${zoneData.lossPercentage}% of zone supply`} 
            icon="💧" 
            color={getLossColor(parseFloat(zoneData.lossPercentage))}
          />
          <StatusCard 
            title="Efficiency Rating" 
            value={`${(100 - parseFloat(zoneData.lossPercentage)).toFixed(1)}%`}
            subtitle="Water properly accounted for" 
            icon="⭐" 
            color={getEfficiencyColor(100 - parseFloat(zoneData.lossPercentage))}
          />
        </div>
        
        <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
          <Card 
            title="Zone Consumption Balance" 
            subtitle="Comparing bulk intake to end-user consumption"
          >
            <div className="h-80">
              <ResponsiveContainer width="100%" height="100%">
                <PieChart>
                  <Pie
                    data={[
                      { name: 'Individual Consumption', value: zoneData.individual },
                      { name: 'Unaccounted Water', value: zoneData.loss }
                    ]}
                    cx="50%"
                    cy="50%"
                    labelLine={false}
                    label={({ name, percent }) => `${name}: ${(percent * 100).toFixed(1)}%`}
                    outerRadius={80}
                    fill="#8884d8"
                    dataKey="value"
                  >
                    <Cell fill={theme.success} />
                    <Cell fill={theme.danger} />
                  </Pie>
                  <Tooltip formatter={(value) => formatNumber(value)} />
                </PieChart>
              </ResponsiveContainer>
            </div>
          </Card>
          
          <Card 
            title="Monthly Efficiency Trend" 
            subtitle={`Zone ${activeZone} efficiency over time`}
          >
            <div className="h-80">
              <ResponsiveContainer width="100%" height="100%">
                <LineChart data={months.slice(0, -1).map(month => {
                  // Simulate varying efficiency by month
                  const baseEfficiency = 100 - parseFloat(zoneData.lossPercentage);
                  const randomVariation = Math.sin(months.indexOf(month) * 0.8) * 5;
                  return {
                    month,
                    efficiency: baseEfficiency + randomVariation,
                    target: 85
                  };
                })}>
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis dataKey="month" />
                  <YAxis domain={[0, 100]} tickFormatter={(value) => `${value}%`}>
                    <Label value="Efficiency (%)" angle={-90} position="insideLeft" style={{ textAnchor: 'middle' }} />
                  </YAxis>
                  <Tooltip formatter={(value) => `${value.toFixed(1)}%`} />
                  <Legend />
                  <Line type="monotone" dataKey="efficiency" name="Actual Efficiency" stroke={theme.primary} strokeWidth={2} />
                  <Line type="monotone" dataKey="target" name="Target" stroke={theme.success} strokeDasharray="5 5" />
                </LineChart>
              </ResponsiveContainer>
            </div>
          </Card>
        </div>
        
        <div className="grid grid-cols-1 lg:grid-cols-3 gap-6">
          <Card 
            title="End-User Breakdown" 
            subtitle={`Zone ${activeZone} consumption categories`}
            className="lg:col-span-2"
          >
            <div className="h-80">
              <ResponsiveContainer width="100%" height="100%">
                <BarChart 
                  data={[
                    { name: 'Residential', value: zoneData.name === 'Zone 3A' || zoneData.name === 'Zone 3B' ? 
                      zoneData.individual * 0.7 : zoneData.individual * 0.5 },
                    { name: 'Common Areas', value: zoneData.individual * 0.15 },
                    { name: 'Irrigation', value: zoneData.name === 'Zone 5' ? 
                      zoneData.individual * 0.25 : zoneData.individual * 0.1 },
                    { name: 'Retail', value: zoneData.name === 'Zone FM' || zoneData.name === 'Zone VS' ? 
                      zoneData.individual * 0.25 : zoneData.individual * 0.05 }
                  ].sort((a, b) => b.value - a.value)}
                  layout="vertical"
                  margin={{ top: 5, right: 30, left: 20, bottom: 5 }}
                >
                  <CartesianGrid strokeDasharray="3 3" />
                  <XAxis type="number">
                    <Label value="Volume (m³)" position="insideBottom" style={{ textAnchor: 'middle' }} />
                  </XAxis>
                  <YAxis type="category" dataKey="name" />
                  <Tooltip formatter={(value) => formatNumber(value)} />
                  <Legend />
                  <Bar dataKey="value" name="Consumption" fill={theme.info}>
                    {['Residential', 'Common Areas', 'Irrigation', 'Retail'].map((entry, index) => (
                      <Cell key={`cell-${index}`} fill={CHART_COLORS[index % CHART_COLORS.length]} />
                    ))}
                  </Bar>
                </BarChart>
              </ResponsiveContainer>
            </div>
          </Card>
          
          <Card 
            title="Zone Metrics"
            subtitle={`Key performance indicators`}
          >
            <div className="space-y-4 h-80 overflow-y-auto">
              <div>
                <div className="flex justify-between mb-1">
                  <span className="text-sm font-medium text-gray-700">Water Accountability</span>
                  <span className="text-sm font-medium">{(100 - parseFloat(zoneData.lossPercentage)).toFixed(1)}%</span>
                </div>
                <ProgressBar 
                  value={100 - parseFloat(zoneData.lossPercentage)} 
                  color={getEfficiencyColor(100 - parseFloat(zoneData.lossPercentage))} 
                />
                <p className="text-xs text-gray-500 mt-1">
                  {parseFloat(zoneData.lossPercentage) < 10 ? 
                    'Excellent accountability' : 
                    parseFloat(zoneData.lossPercentage) < 20 ?
                    'Good accountability, minor improvements needed' :
                    'Poor accountability, requires investigation'}
                </p>
              </div>
              
              <div className="pt-3">
                <h4 className="text-sm font-medium text-gray-700 mb-2">Loss Comparison to Other Zones</h4>
                <div className="space-y-2">
                  {prepareZoneLossData().slice(0, 4).map((z, idx) => (
                    <div key={idx} className="flex items-center gap-2">
                      <div className="w-3 h-3 rounded-full" style={{ backgroundColor: CHART_COLORS[idx % CHART_COLORS.length] }}></div>
                      <span className="text-xs">{z.name}</span>
                      <div className="flex-1 h-2 bg-gray-200 rounded-full">
                        <div 
                          className="h-2 rounded-full" 
                          style={{ 
                            width: `${parseFloat(z.lossPercentage)}%`, 
                            backgroundColor: getLossColor(parseFloat(z.lossPercentage)) 
                          }}
                        ></div>
                      </div>
                      <span className="text-xs font-medium">{z.lossPercentage}%</span>
                    </div>
                  ))}
                </div>
              </div>
              
              <div className="pt-3">
                <h4 className="text-sm font-medium text-gray-700 mb-2">Recommended Actions</h4>
                <ul className="text-xs text-gray-600 space-y-1">
                  {zoneData.name === 'Zone 5' ? (
                    <>
                      <li className="flex items-start gap-1">
                        <span className="text-red-500">⚠️</span>
                        <span>Critical: Schedule immediate inspection of main distribution pipes</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-red-500">⚠️</span>
                        <span>Verify irrigation system operation and controls</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-yellow-500">⚠️</span>
                        <span>Review sub-meter coverage for potential gaps</span>
                      </li>
                    </>
                  ) : parseFloat(zoneData.lossPercentage) > 20 ? (
                    <>
                      <li className="flex items-start gap-1">
                        <span className="text-red-500">⚠️</span>
                        <span>High priority: Conduct zone pressure test to locate leaks</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-yellow-500">⚠️</span>
                        <span>Check bulk meter calibration</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-blue-500">ℹ️</span>
                        <span>Review individual meter data for anomalies</span>
                      </li>
                    </>
                  ) : (
                    <>
                      <li className="flex items-start gap-1">
                        <span className="text-green-500">✓</span>
                        <span>Zone performing well, continue regular monitoring</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-blue-500">ℹ️</span>
                        <span>Schedule routine meter calibration</span>
                      </li>
                      <li className="flex items-start gap-1">
                        <span className="text-blue-500">ℹ️</span>
                        <span>Document best practices for other zones</span>
                      </li>
                    </>
                  )}
                </ul>
              </div>
            </div>
          </Card>
        </div>
      </div>
    );
  };
  
  // Action Plan View
  const ActionPlanView = () => (
    <div className="space-y-6">
      <div className="bg-white rounded-lg shadow-md p-4">
        <h2 className="text-xl font-bold text-gray-800 mb-2">Water Loss Reduction Action Plan</h2>
        <p className="text-sm text-gray-600">
          Implementation status of identified improvement initiatives
        </p>
      </div>
      
      <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
        <StatusCard 
          title="Critical Actions" 
          value="2"
          subtitle="Highest priority items requiring immediate attention" 
          icon="🔴" 
          color={theme.danger}
        />
        <StatusCard 
          title="Overall Progress" 
          value="45%"
          subtitle="Combined completion percentage of all actions" 
          icon="📊" 
          color={theme.primary}
        />
        <StatusCard 
          title="Completed Actions" 
          value="1"
          subtitle="Successfully implemented improvements" 
          icon="✅" 
          color={theme.success}
        />
      </div>
      
      <Card 
        title="Action Items" 
        subtitle="Current status of all improvement initiatives"
      >
        <div className="overflow-x-auto">
          <table className="min-w-full divide-y divide-gray-200">
            <thead className="bg-gray-50">
              <tr>
                <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Action
                </th>
                <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Priority
                </th>
                <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Status
                </th>
                <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Deadline
                </th>
                <th scope="col" className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                  Progress
                </th>
              </tr>
            </thead>
            <tbody className="bg-white divide-y divide-gray-200">
              {actionPlanData.map((action) => (
                <tr key={action.id}>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="text-sm font-medium text-gray-900">{action.title}</div>
                    <div className="text-xs text-gray-500">{action.description}</div>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <span 
                      className={`px-2 py-1 inline-flex text-xs leading-5 font-semibold rounded-full ${
                        action.priority === 'Critical' ? 'bg-red-100 text-red-800' :
                        action.priority === 'High' ? 'bg-orange-100 text-orange-800' :
                        'bg-blue-100 text-blue-800'
                      }`}
                    >
                      {action.priority}
                    </span>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <span 
                      className={`px-2 py-1 inline-flex text-xs leading-5 font-semibold rounded-full ${
                        action.status === 'Completed' ? 'bg-green-100 text-green-800' :
                        action.status === 'In Progress' ? 'bg-blue-100 text-blue-800' :
                        action.status === 'Pending' ? 'bg-gray-100 text-gray-800' :
                        'bg-yellow-100 text-yellow-800'
                      }`}
                    >
                      {action.status}
                    </span>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                    {action.deadline}
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="w-full bg-gray-200 rounded-full h-2.5 mb-1">
                      <div 
                        className="h-2.5 rounded-full" 
                        style={{ 
                          width: `${action.progress}%`, 
                          backgroundColor: action.priority === 'Critical' ? theme.danger :
                                          action.priority === 'High' ? theme.warning :
                                          theme.info
                        }}
                      ></div>
                    </div>
                    <div className="text-xs text-gray-500">{action.progress}% complete</div>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </Card>
      
      <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
        <Card 
          title="Zone Improvement Priorities" 
          subtitle="Focus areas for loss reduction"
        >
          <div className="h-80">
            <ResponsiveContainer width="100%" height="100%">
              <BarChart 
                data={prepareZoneLossData().map(zone => ({
                  name: zone.name,
                  value: parseFloat(zone.lossPercentage),
                  priority: parseFloat(zone.lossPercentage) > 40 ? 'Critical' :
                            parseFloat(zone.lossPercentage) > 20 ? 'High' : 'Medium'
                }))}
                margin={{ top: 20, right: 30, left: 20, bottom: 5 }}
                layout="vertical"
              >
                <CartesianGrid strokeDasharray="3 3" />
                <XAxis type="number" tickFormatter={(value) => `${value}%`}>
                  <Label value="Loss Percentage" position="insideBottom" style={{ textAnchor: 'middle' }} />
                </XAxis>
                <YAxis type="category" dataKey="name" />
                <Tooltip formatter={(value) => `${value}%`} />
                <Legend />
                <Bar dataKey="value" name="Loss Percentage">
                  {prepareZoneLossData().map((entry, index) => (
                    <Cell 
                      key={`cell-${index}`} 
                      fill={parseFloat(entry.lossPercentage) > 40 ? theme.danger :
                            parseFloat(entry.lossPercentage) > 20 ? theme.warning :
                            theme.success} 
                    />
                  ))}
                </Bar>
              </BarChart>
            </ResponsiveContainer>
          </div>
        </Card>
        
        <Card 
          title="Implementation Timeline" 
          subtitle="Action plan schedule"
        >
          <div className="h-80 overflow-y-auto">
            <div className="relative">
              <div className="absolute h-full w-px bg-blue-200 left-4"></div>
              
              {[
                { date: "Feb 15, 2025", title: "Technical Support Protocol Established", status: "Completed" },
                { date: "Mar 10, 2025", title: "Zone 5 System Check", status: "Scheduled", note: "Critical priority - addressing largest loss area" },
                { date: "Mar 15, 2025", title: "Data Review Meeting with Tadoom", status: "In Progress", note: "Analyzing smart meter data discrepancies" },
                { date: "Mar 30, 2025", title: "Irrigation System Assessment", status: "Pending", note: "Focusing on Zones 5 and 8" },
                { date: "Apr 15, 2025", title: "Zone 3A & 3B Investigation", status: "Pending", note: "Review of meter placement with Tadoom" },
                { date: "Apr 30, 2025", title: "Monthly Progress Review", status: "Scheduled", note: "First monthly review with stakeholders" },
                { date: "May 15, 2025", title: "Performance Tracking Implementation", status: "Pending", note: "Setup of automated monitoring system" },
                { date: "May 30, 2025", title: "Service Standards Definition", status: "Pending", note: "Establishing response time protocols" }
              ].map((item, index) => (
                <div key={index} className="ml-12 mb-6 relative">
                  <div className={`absolute -left-12 w-8 h-8 rounded-full flex items-center justify-center ${
                    item.status === 'Completed' ? 'bg-green-500' :
                    item.status === 'In Progress' ? 'bg-blue-500' :
                    item.status === 'Scheduled' ? 'bg-yellow-500' :
                    'bg-gray-300'
                  } text-white z-10`}>
                    {item.status === 'Completed' ? '✓' :
                     item.status === 'In Progress' ? '→' :
                     item.status === 'Scheduled' ? '!' : '?'}
                  </div>
                  <div className="text-xs text-gray-500">{item.date}</div>
                  <div className="font-medium">{item.title}</div>
                  <div className="text-sm text-gray-600 mt-1">{item.note}</div>
                  <div className={`text-xs ${
                    item.status === 'Completed' ? 'text-green-600' :
                    item.status === 'In Progress' ? 'text-blue-600' :
                    item.status === 'Scheduled' ? 'text-yellow-600' :
                    'text-gray-600'
                  } mt-1 font-medium`}>
                    {item.status}
                  </div>
                </div>
              ))}
            </div>
          </div>
        </Card>
      </div>
    </div>
  );
  
  return (
    <div className="min-h-screen bg-gray-100">
      {/* Header */}
      <header className="bg-white shadow-sm border-b border-gray-200">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
          <div className="flex justify-between items-center flex-wrap gap-4">
            <div>
              <h1 className="text-2xl font-bold text-gray-900 flex items-center gap-2">
                <span className="text-blue-600">💧</span> Muscat Bay Water Management
              </h1>
              <p className="text-sm text-gray-500">Water Consumption Analysis and Loss Management • 2024</p>
            </div>
            <div className="flex items-center gap-4">
              <div className="text-sm text-gray-600 flex items-center gap-1">
                <span className="font-medium">Total Water:</span>
                <span className="text-blue-600 font-bold">{formatNumber(waterData.mainBulk.total)} m³</span>
              </div>
              <div className="text-sm text-gray-600 flex items-center gap-1">
                <span className="font-medium">Total Loss:</span>
                <span className="text-red-600 font-bold">{formatNumber(lossData.mainToIndividual.total)} m³</span>
                <span className="text-red-600 font-bold">({lossData.mainToIndividual.percentage}%)</span>
              </div>
              <button className="px-3 py-1.5 bg-blue-600 text-white rounded-md text-sm font-medium hover:bg-blue-700 transition-colors flex items-center gap-1">
                <span>Export Report</span>
              </button>
            </div>
          </div>
        </div>
      </header>
      
      {/* Navigation Tabs */}
      <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4">
        <div className="flex flex-wrap gap-2 mb-6">
          <TabButton 
            label="Executive Summary" 
            active={activeView === 'executive'} 
            onClick={() => setActiveView('executive')}
            icon="📊"
          />
          <TabButton 
            label="Consumption Analysis" 
            active={activeView === 'consumption'} 
            onClick={() => setActiveView('consumption')}
            icon="🌊"
          />
          <TabButton 
            label="Loss Analysis" 
            active={activeView === 'losses'} 
            onClick={() => setActiveView('losses')}
            icon="💧"
          />
          <TabButton 
            label="Zone Analysis" 
            active={activeView === 'zones'} 
            onClick={() => setActiveView('zones')}
            icon="🏙️"
          />
          <TabButton 
            label="Action Plan" 
            active={activeView === 'actions'} 
            onClick={() => setActiveView('actions')}
            icon="📝"
          />
        </div>
      </div>
      
      {/* Main Content */}
      <main className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 pb-12">
        {activeView === 'executive' && <ExecutiveView />}
        {activeView === 'consumption' && <ConsumptionView />}
        {activeView === 'losses' && <LossView />}
        {activeView === 'zones' && <ZoneView />}
        {activeView === 'actions' && <ActionPlanView />}
      </main>
      
      {/* Footer */}
      <footer className="bg-white border-t border-gray-200 py-4">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex justify-between items-center">
            <p className="text-sm text-gray-500">
              Prepared by: Abdulrahim Al Balushi | Last updated: February 27, 2025
            </p>
            <div className="text-sm text-gray-500">
              Muscat Bay Water Management Dashboard v1.0
            </div>
          </div>
        </div>
      </footer>
    </div>
  );
};

export default MuscatBayDashboard;
