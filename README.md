# Red Hat Ansible Automation Platform - OpenShift Compatibility Matrix

[![GitHub last commit](https://img.shields.io/github/last-commit/lennysh/aap-openshift-compatibility-matrix.svg)](https://github.com/lennysh/aap-openshift-compatibility-matrix/commits/main) [![GitHub license](https://img.shields.io/github/license/lennysh/aap-openshift-compatibility-matrix.svg)](https://github.com/lennysh/aap-openshift-compatibility-matrix/blob/main/LICENSE) [![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)](https://github.com/lennysh/aap-openshift-compatibility-matrix/pulls)

This repository provides a centralized, community-driven tracker for Red Hat Ansible Automation Platform (AAP) operator and component versions, specifically for deployments on OpenShift. It aims to offer a clear and quick reference for mapping AAP operator releases to their corresponding component versions, such as Controller, EDA, Hub, and more.

## 📋 Compatibility Tables

For easy viewing, the raw data has been converted into user-friendly Markdown tables, which are generated from the central CSV file.

* [**Red Hat Ansible Automation Platform 2.4**](./AAP_24.md)
* [**Red Hat Ansible Automation Platform 2.5**](./AAP_25.md)

## ⚙️ How It Works

The core of this repository is a simple, automated workflow designed for clarity and easy maintenance.

1.  **Raw Data**: All version information is maintained in the **[data/AAP_ALL.csv](./data/AAP_ALL.csv)** file. This is the single source of truth for the entire repository and the only file that should be edited manually.

2.  **Conversion Script**: A bash script (`csv2md.sh`) reads the `data/AAP_ALL.csv` file. This script can filter rows, remove columns, and format URLs into clickable links.

3.  **Markdown Output**: The script processes the data and generates the two Markdown files (`AAP_24.md` and `AAP_25.md`), creating clean, readable tables.

### Automation Example

The Markdown files are generated using the `csv2md.sh` script included in this repository. For example, to regenerate the AAP 2.4 table, the following command is used:

```bash
./scripts/csv2md.sh \
  -t "Red Hat Ansible Automation Platform 2.4 - OpenShift Operator Component versions" \
  -F "AAP Ver.=2.4" \
  -R "AAP Ver." \
  ./data/AAP_ALL.csv > AAP_24.md
```

This command creates a titled table, filters the CSV to only include rows where `AAP Ver.` is `2.4`, and removes the now-redundant "AAP Ver." column before saving the output.

## 🤝 Contributing

Found a mistake or have an update for a new release? Contributions are highly encouraged!

To contribute, please **submit a pull request with your changes to the `data/AAP_ALL.csv` file only**. Do not edit the Markdown files directly, as they are overwritten by the automation script. Once your pull request is merged, the script can be re-run to update the tables.

## ✨ Contributors

A big thank you to all the contributors who have helped improve this project! You can see a full list of everyone who has contributed on the [contributors page](https://github.com/lennysh/aap-openshift-compatibility-matrix/graphs/contributors).

<a href = "https://github.com/lennysh/aap-openshift-compatibility-matrix/graphs/contributors">
  <img src = "https://contrib.rocks/image?repo=lennysh/aap-openshift-compatibility-matrix"/>
</a>

## ✍️ Authors

* [CastawayEGR](https://github.com/CastawayEGR) *(Deserves most of the credit!)*
* [LennySh](https://github.com/lennysh)

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.