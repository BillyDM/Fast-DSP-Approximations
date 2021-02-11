## xorshift32
license: Public Domain

source: https://www.airwindows.com/ditherfloat/

Rust:
```rust
pub struct FastRng {
  fpd: u32,
}

impl Default for FastRng {
  fn default() -> FastRng {
    FastRng { fpd: 17 }
  }
}

impl FastRng {
  pub fn new(mut seed: u32) -> FastRng {
    // seed cannot be zero
    if seed == 0 { seed = 17; }
    FastRng { fpd: seed }
  }
  
  #[inline]
  pub fn gen_u32(&mut self) -> u32 {
    self.fpd ^= self.fpd << 13;
    self.fpd ^= self.fpd >> 17;
    self.fpd ^= self.fpd << 5;
  }
  
  #[inline]
  pub fn gen_f32(&mut self) -> f32 {
    self.gen_u32() as f32 / std::u32::MAX as f32;
  }
  
  #[inline]
  pub fn gen_f64(&mut self) -> f64 {
    self.gen_u32() as f64 / std::u32::MAX as f64;
  }
}
```
