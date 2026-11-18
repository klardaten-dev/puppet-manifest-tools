# Changelog

## 1.1.0
EPP template support (`*.epp`):
- Highlighting of template tags and of the Puppet code inside them.
- The code of all tags is parsed as one Puppet program, like Puppet does, so blocks spanning several tags
  (`<% $list.each |$x| { -%> ... <% } -%>`, `if`/`elsif`/`else`, `case`) work.
- Legacy facts (`<%= $::fqdn %>`, `$facts['osfamily']`) are flagged with the one-click Puppet 8 fix, as in manifests.
- Template parameters: go to declaration, find usages, completion.
- Errors for unclosed tags and a parameter tag that is not at the start of the template.
- Comment with block comment in template text inserts `<%# %>`.

Manifest fixes (found in 13 more modules, now tested on ~1,000 manifests and 260 templates from 51 modules):
- `Resource['type'] { $title: ... }` declarations and resource declarations inside arrays (`-> [ foo { 'a': }, bar { 'b': } ]`) parse correctly.
- `file` `target` accepts any link target (was wrongly limited to `notlink`).

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
