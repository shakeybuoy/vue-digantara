<template>
  <div class="wrapper">
    <div class="hero-image">
      <img
        src="https://cdn.dribbble.com/userupload/3862984/file/original-e3a325ffc58fce469685e7ed9132c1f7.jpg?resize=752x"
        alt="hero-image"
      />
      <h2>New-age Space Cartographers</h2>
      <p>
        Safeguarding the future of space operations through comprehensive space
        situational awareness infrastructure.
      </p>
    </div>
  </div>
</template>

<style>
.wrapper {
  width: 50%;
}
.hero-image {
  text-align: center;
}
.hero-image img {
  width: 90%;
  border-radius: 10px;
  margin-bottom: 20px;
}
.hero-image h2 {
  font-size: 2.2rem;
  font-weight: 800;
  color: white;
}
.hero-image p {
  margin-top: 10px;
  font-size: 1.2rem;
}
@media only screen and (max-width: 1023px) {
  .wrapper {
    display: none;
    width: 0%;
  }
}
</style>
