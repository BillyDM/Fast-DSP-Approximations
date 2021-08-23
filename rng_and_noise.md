## xorshift32 RNG
license: Public Domain

modified algorithm from: https://www.airwindows.com/ditherfloat/

Rust:
```rust
pub struct XOrShift32Rng {
  fpd: i32,
}

impl Default for XOrShift32Rng {
  fn default() -> XOrShift32Rng {
    XOrShift32Rng { fpd: 17 }
  }
}

impl XOrShift32Rng {
  pub fn new(mut seed: i32) -> XOrShift32Rng {
    // seed cannot be zero
    if seed == 0 { seed = 17; }
    XOrShift32Rng { fpd: seed }
  }
  
  /// Generates a random `i32`
  #[inline]
  pub fn gen_i32(&mut self) -> i32 {
    self.fpd ^= self.fpd << 13;
    self.fpd ^= self.fpd >> 17;
    self.fpd ^= self.fpd << 5;
    self.fpd
  }
  
  /// Generates a random `f32` in the range `[-1.0, 1.0]`
  #[inline]
  pub fn gen_f32(&mut self) -> f32 {
    f32::from(self.gen_i32()) / f32::from(std::i32::MAX)
  }
  
  /// Generates a random `f64` in the range `[-1.0, 1.0]`
  #[inline]
  pub fn gen_f64(&mut self) -> f64 {
    f64::from(self.gen_i32()) / f64::from(std::i32::MAX)
  }
  
  /// Generates a random `f32` in the range `[0.0, 1.0]`
  #[inline]
  pub fn gen_f32_normalized(&mut self) -> f32 {
    (f32::from(self.gen_i32()) / (2.0 * f32::from(std::i32::MAX))) + 0.5
  }
  
  /// Generates a random `f64` in the range `[0.0, 1.0]`
  #[inline]
  pub fn gen_f64_normalized(&mut self) -> f64 {
    (f64::from(self.gen_i32()) / (2.0 * f64::from(std::i32::MAX))) + 0.5
  }
}
```
