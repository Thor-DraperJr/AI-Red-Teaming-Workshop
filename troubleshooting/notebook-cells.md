

```python
# Cell 11: Advanced scan + inline stderr ERROR capture (standardized naming & writable output dir)
from datetime import datetime, timezone
import sys, io, re, contextlib, time, json
from pathlib import Path
import pathlib  # for consistent writable directory handling

#### Debugger (capture + extract ERROR lines) ####
error_re = re.compile(r'^ERROR:.*', re.IGNORECASE)
class _Cap(contextlib.AbstractContextManager):
    def __enter__(self):
        self._orig = sys.stderr; self.buf = io.StringIO(); sys.stderr = self; return self
    def write(self,d): self.buf.write(d); self._orig.write(d)
    def flush(self): self._orig.flush()
    def __exit__(self,*a): sys.stderr = self._orig
#### End Debugger (setup) ####

scan_ts = datetime.now(timezone.utc).strftime('%H%M%S')
scan_label = "Advanced"
scan_name = f"{scan_label}-Scan-{scan_ts}"

# Use same writable directory pattern as earlier scans (fallback to home if attribute missing)
_writable_base = pathlib.Path(getattr(red_team.file_manager, 'base_output_dir', Path.home()))
_writable_base.mkdir(parents=True, exist_ok=True)
output_path = _writable_base / f"{scan_label.lower()}_scan-{scan_ts}.json"

print(f"Running {scan_name} -> {output_path}")
with _Cap() as cap:  # debugger active
    advanced_scan_result = await advanced_red_team.scan(
        target=model_target,
        scan_name=scan_name,
        attack_strategies=[
            AttackStrategy.EASY,
            AttackStrategy.MODERATE,
            AttackStrategy.Flip,
            AttackStrategy.CharSwap,
            AttackStrategy.UnicodeConfusable,
            AttackStrategy.Leetspeak,
            AttackStrategy.Url,
            AttackStrategy.Compose([AttackStrategy.Base64, AttackStrategy.ROT13]),
        ],
        output_path=str(output_path),
    )
    sys.stderr.flush(); time.sleep(0.2)

#### Debugger (ERROR extraction/print) ####
errs = [ln.strip() for ln in cap.buf.getvalue().splitlines() if error_re.match(ln)]
print(f"Completed {scan_name} -> {output_path}")
print("No ERROR lines captured." if not errs else f"{len(errs)} ERROR line(s):\n" + "\n".join(errs))
```
