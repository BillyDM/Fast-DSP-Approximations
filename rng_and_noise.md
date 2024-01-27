# XOrShift32 Random Number Generator
A very fast random number generator that can be used to generate decent quality white noise.

* license: Public Domain
* original algorithm from https://www.airwindows.com/ditherfloat/, adapted by Billy Messenger

C++:
```c++
#include <cstdint>

class XOrShift32Rng {
  uint32_t m_fpd;

public:
  XOrShift32Rng(uint32_t seed = 17) {
    // seed cannot be zero
    m_fpd = seed == 0 ? 17 : seed;
  }

  // Generates a random `uint32_t`
  inline uint32_t gen_uint32() {
    m_fpd ^= m_fpd << 13;
    m_fpd ^= m_fpd >> 17;
    m_fpd ^= m_fpd << 5;
    return m_fpd;
  }

  // Generates a random float in the range `[0.0, 1.0]`
  inline float gen_float() {
    return static_cast<float>( this->gen_uint32() ) * (1.0f / 4294967295.0f);
  }

  // Generates a random double in the range `[0.0, 1.0]`
  inline double gen_double() {
    return static_cast<double>( this->gen_uint32() ) * (1.0 / 4294967295.0);
  }

  // Generates a random float in the range `[-1.0, 1.0]`
  inline double gen_noise_float() {
    return static_cast<float>( this->gen_uint32() ) * (2.0f / 4294967295.0f) - 1.0f;
  }

  // Generates a random double in the range `[-1.0, 1.0]`
  inline double gen_noise_double() {
    return static_cast<double>( this->gen_uint32() ) * (2.0 / 4294967295.0) - 1.0;
  }
};
```

Rust:
```rust
pub struct XOrShift32Rng {
  fpd: u32,
}

impl Default for XOrShift32Rng {
  fn default() -> XOrShift32Rng {
    XOrShift32Rng { fpd: 17 }
  }
}

impl XOrShift32Rng {
  pub fn new(mut seed: u32) -> XOrShift32Rng {
    // seed cannot be zero
    if seed == 0 { seed = 17; }
    XOrShift32Rng { fpd: seed }
  }
  
  /// Generates a random `u32`
  #[inline]
  pub fn gen_u32(&mut self) -> u32 {
    self.fpd ^= self.fpd << 13;
    self.fpd ^= self.fpd >> 17;
    self.fpd ^= self.fpd << 5;
    self.fpd
  }
  
  /// Generates a random `f32` in the range `[0.0, 1.0]`
  #[inline]
  pub fn gen_f32(&mut self) -> f32 {
    self.gen_u32() as f32 * (1.0 / 4_294_967_295.0)
  }
  
  /// Generates a random `f64` in the range `[0.0, 1.0]`
  #[inline]
  pub fn gen_f64(&mut self) -> f64 {
    f64::from(self.gen_u32()) * (1.0 / 4_294_967_295.0)
  }

  /// Generates a random `f32` in the range `[-1.0, 1.0]`
  #[inline]
  pub fn gen_noise_f32(&mut self) -> f32 {
    self.gen_u32() as f32 * (2.0 / 4_294_967_295.0) - 1.0;
  }

  /// Generates a random `f64` in the range `[-1.0, 1.0]`
  #[inline]
  pub fn gen_noise_f64(&mut self) -> f64 {
    f64::from(self.gen_u32()) * (2.0 / 4_294_967_295.0) - 1.0;
  }
}
```
