# Changelog
All notable changes to the project will be documented in this file.

## [Unreleased]


## [0.3.0] - 2018-05-29
### Added
- --b flag to specify a specific block integer
- Additional code commenting and error messaging
- Release binaries to allow for program run without Cargo installed
- Contract address check to ensure event handling comes from the correct contracts instance

### Changed
- Moved counter code to separate module at counter.rs and refactored counter code
- -v option extended; displays full lists of participating and abstaining voters for each ballot

### Fixed
- Switched to `VotingKeyChanged` event to track the current validator set and confirm a mining key is finalized rather than `InitializeChange` event, which was not always finalized.


## [0.2.1] - 2018-05-24

### Fixed
- Web3 compilation issue.

## [0.2.0] - 2018-05-21
### Added
-  -p flag to show limited time periods in which ballots are counted

### Changed
- Only show current validators. Validators who have been removed are no longer displayed.

### Fixed
- Fixed server filter registration to allow for use with load-balanced servers and compatibility with https://core.poa.network 


## [0.1.0] - 2018-05-19
### Added
- Initial implementation
- GPL3 License
- Enabled Travis CI
- ABI and contract address files updated from [poa-chain-spec](https://github.com/poanetwork/poa-chain-spec)
- Use Ethabi Contract
- Reference to [RP9 specifications](https://github.com/poanetwork/RFC/issues/9)
- Enabled build scripts
- Checks for node sychonization / error messages if nodes are not synced
- Parsing validator addresses for display

### Removed
- MIT license

[0.3.0]: https://github.com/poanetwork/poa-ballot-stats/releases/tag/0.3.0
[0.2.1]: https://github.com/poanetwork/poa-ballot-stats/releases/tag/0.2.1
[0.2.0]: https://github.com/poanetwork/poa-ballot-stats/releases/tag/0.2.0
[0.1.0]: https://github.com/poanetwork/poa-ballot-stats/releases/tag/0.1.0
