# Publication and validation checklist

## Repository contents

- [x] Repository name and description selected
- [x] Documentation scaffold, README, addressing plan, security policy, validation plan created
- [ ] Replace diagram placeholder with final exported topology
- [ ] Add authentic `.pkt` file and reopen it successfully
- [ ] Add real sanitized core, edge, ISP, and six access-switch configurations
- [ ] Confirm all addresses, external test subnets and routing details against `.pkt`

## Troubleshooting and validation

- [ ] Troubleshoot and record outcome of SSH to `192.168.10.97`
- [ ] Capture VLAN membership / trunk / SVI / routing evidence
- [ ] Capture DHCP relay and per-VLAN lease evidence
- [ ] Capture DNS and internal web evidence
- [ ] Capture external connectivity and NAT/PAT evidence
- [ ] Capture guest isolation and Sales-to-Finance denial evidence
- [ ] Capture server outbound and Administration/Finance ACL evidence
- [ ] Archive exact ACL rules and interface directions
- [ ] Retest authorized flows to avoid false-positive ACL conclusions
- [ ] Record final results in `evidence/test-results/validation-matrix.csv`

## Publication readiness

- [ ] Check files and screenshots contain no secrets
- [ ] Audit README to distinguish reported features from verified results
- [ ] Ensure there are no broken relative image/document links
- [ ] Push to GitHub (option: private while still in progress)
- [ ] If final tests pass, switch to public and update status to *Validated*
- [ ] Only after publication, prepare the LinkedIn post with real screenshots and repository URL
