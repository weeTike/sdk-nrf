# nrf_cc3xx_platform is not a driver in the old APIs. Once ctr_drbg/hmac_drbg
# gets real driver API support then the placement here makes more sense
if(TARGET nrf_cc3xx_platform)
  add_subdirectory(nrf_cc3xx_platform)
endif()

if(CONFIG_BUILD_WITH_TFM)
  # When TF-M is used the Crypto driver is in the TF-M image so we
  # don't need to add the sources from this non-secure image
else()
  if(CONFIG_PSA_CRYPTO_DRIVER_CC3XX)
	add_subdirectory(nrf_cc3xx)
  endif()

  if(CONFIG_PSA_CRYPTO_DRIVER_CRACEN OR (CONFIG_NRF_SECURITY_LEGACY_AND_PSA AND CONFIG_SOC_SERIES_NRF54LX))
	add_subdirectory(cracen)
  endif()
endif()

add_subdirectory(zephyr)

if(CONFIG_PSA_CRYPTO_DRIVER_OBERON OR CONFIG_OBERON_BACKEND)
  add_subdirectory(nrf_oberon)
endif()
