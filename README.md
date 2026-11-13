# Team 2: Asset, Configuration & Change Management
CADT-DigitalGov & Pay capstone. See `docs/` for threat model, control mapping, and interface agreements.

| Folder | Owner | Controls | What it does |
|---|---|---|---|
| asset-discovery/ | Member 1 | CM-8, CIS 1 | Scanner, CMDB, `/api/v1/assets` API |
| hardening/ | Member 2 | CM-6, CIS 4 | Ansible CIS role + daily drift audit |
| app-control/ | Member 3 | CM-7(5), CIS 2.5 | fapolicyd allowlisting, provisioning hook |
| container-security/ | Member 4 | CM-14, CIS 2.7 | Cosign signing, Kyverno admission policy |
| .github/workflows/ | Member 4 | CM-3 | CI: lint, tests, Trivy |
| docs/ | Member 5 | all | Compliance and QA documents |

## Quick start
```
cd asset-discovery && pip install -r requirements.txt && pytest
sudo python scanner.py --subnet <T6-approved-CIDR>
ASSET_API_KEY=change-me uvicorn api.app:app --port 8000
ansible-playbook -i inventory.ini hardening/playbooks/cis_level2.yml
sudo python hardening/audit/cis_audit.py --hostname <host> --post http://localhost:8000 --api-key change-me
sudo app-control/fapolicyd/setup.sh
container-security/cosign/sign_and_verify.sh <image>
```

## Rules
No direct pushes to `main`. Pull requests need 2 approvals and passing CI. Never commit private keys.