# data layer – what's left

Done so far:
- `loaders.py` – `DataLoader` base + `StationDataLoader` + `VehicleDataLoader` (returns dicts for now)
- `persistence.py` – `SnapshotManager` save/load (JSON snapshot)
- `data/stations.csv` + `data/vehicles.csv` – sample data for testing

---

## Still to do

### Waiting on Role 4 (domain models)
- Replace the `TODO` stubs in `loaders.py` — construct real `Station` / `Vehicle` subclass objects instead of plain dicts
- Replace the `TODO` stubs in `persistence.py` — restore `Station`, `Vehicle`, `User`, `Ride` objects from snapshot

### Waiting on Role 3 (FleetManager)
- Wire `SnapshotManager.load()` into the app startup so state is restored on restart
- Wire `SnapshotManager.save()` into `end_ride` (and maybe a background periodic save)

### Tests (can start now / partially)
- `tests/test_loaders.py` — parse CSVs, check types, test missing file error
- `tests/test_persistence.py` — save a dict snapshot, reload it, assert round-trip consistency
- Integration test (later): load CSVs → do a ride → save → reload → assert state matches

### Nice to have (if time permits)
- `VehicleDataLoader` validation: warn if `charge_pct` is set for a `BICYCLE`
- `SnapshotManager` backup: keep last N snapshots so a bad write doesn't wipe state
