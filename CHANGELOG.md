# Changelog

## 1.0.1
Parser and inspection fixes found by testing against ~790 manifests from 38 popular Puppet modules:
- `if`/`unless`/`case` used as values no longer cause parse errors.
- Function calls with a trailing hash without braces (`epp('t.epp', key => $v)`) parse correctly.
- Keywords as hash keys (`{ environment => ... }`) are accepted.
- Resource overrides (`File['x'] { notify +> ... }`) and capitalized declarations (`Exec { 'title': ... }`) are parsed and checked.
- Custom types written in Ruby (`lib/puppet/type/*.rb`, e.g. archive, firewall, vcsrepo) are no longer reported as missing defined types.
- Clearer message for `file` `ensure` values that Puppet treats as a symlink target; `ensure => false` is accepted.

## 1.0.0
First release: Puppet/OpenVox manifest highlighting, Puppet 8 legacy-fact inspection with quick fix, completion (types, attributes, values, functions, classes, facts), cross-file navigation, inspections, quick documentation.
