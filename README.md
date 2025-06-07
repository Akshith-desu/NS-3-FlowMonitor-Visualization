# NS-3 Network Simulation Analyzer

A lightweight, browser-based tool for parsing NS-3 FlowMonitor XML output and visualizing key performance metrics—throughput, delay, jitter, and packet loss—for TCP/UDP flows.

---

## 🔍 Features

- **XML Parsing**  
  Reads NS-3 `<FlowMonitor>` XML files, extracts per-flow statistics and classifier info.

- **Flows Filtering**  
  Automatically filters to show only application-level TCP flows (destination ports 8080–8089).  
  _(Customize the port range in `parseXML()` if you use different ports.)_

- **Interactive Dashboards**

  - **Network Info**: total flows, protocol, address range
  - **Simulation Summary**: total packets, average throughput, overall loss rate
  - **Metric Cards**: total throughput, max delay, total lost packets, average jitter
  - **Charts**: bar/line/horizontal-bar charts for throughput, delay, loss, jitter
  - **Flow Table**: detailed per-flow breakdown with “Good / Fair / Poor” status

- **Zero Dependencies**  
  Pure HTML/CSS/JS; uses Chart.js.

---

## 📦 Getting Started

### Prerequisites

- Modern web browser (Chrome, Firefox, Edge, Safari)
- NS-3 FlowMonitor XML output (e.g. `scratch/hello_flowmon.xml`)

### Installation

```bash
git clone https://github.com/Akshith-desu/NS-3-FlowMonitor-Visualization.git
cd NS-3-FlowMonitor-Visualization
```

Then open `index.html` in your browser:

- Double-click `index.html`
- Or serve via local HTTP and navigate to `http://localhost:8080/index.html`

---

## 🚀 Usage

1. **Run your NS-3 simulation** with FlowMonitor enabled:

   ```bash
   ./ns3 run scratch/hello      -- --nodes=5 --tcp=2 --traffic=1 --stopTime=45         --sender=0 --receiver=3         --inject=true --injectSender=4 --injectReceiver=2         --injectDelay=0.3 --routing=1 --topology=4
   ```

   hello.cc is the file where the ns-3 code is present

2. **Locate** the generated XML file nd download it.

3. **Open** `index.html` in your browser.

4. **Click** **Choose XML File** and select your FlowMonitor XML.

5. **Explore** the interactive performance dashboard!

---

## ⚙️ Configuration

### Port Range Filter

By default, only flows whose destination port is in 8080–8089 are displayed.  
To change it:

1. Open `index.html` in a code editor.
2. Find in the `parseXML()` function:
   ```js
   const dstPort = parseInt((info.destination || "").split(":").pop(), 10);
   if (isNaN(dstPort) || dstPort < 8080 || dstPort > 8089) {
     return;
   }
   ```
3. Adjust `8080` and `8089` to your desired port range.

### Protocol Support

Currently parses IPv4 flows. To add IPv6 support, duplicate the classifier/parsing logic for `<Ipv6FlowClassifier>` sections in the XML.

---

## 🛠️ File Structure

```text
.
├── index.html        # Main web UI and parser script
└── README.md         # This documentation
```

- **index.html**
  - CSS styling
  - File input & UI skeleton
  - `<script>`:
    - `parseXML()` → build `flowData[]`
    - `displayResults()` → populate cards, charts, table
    - Chart.js configs for throughput, delay, loss, jitter

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch:
   ```bash
   git checkout -b feature/xyz
   ```
3. Commit your changes:
   ```bash
   git commit -am "Add xyz"
   ```
4. Push and open a Pull Request.

---

## 📜 License

This project is licensed under the MIT License.  
See [LICENSE](LICENSE) for details.
