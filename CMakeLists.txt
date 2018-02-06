cmake_minimum_required(VERSION 3.2)

set(
    HUNTER_CACHE_SERVERS
    "https://github.com/forexample/hunter-cache"
    CACHE
    STRING
    "Default cache server"
)

# https://docs.travis-ci.com/user/environment-variables/#Default-Environment-Variables
string(COMPARE EQUAL "$ENV{TRAVIS}" "true" is_travis)

# https://www.appveyor.com/docs/environment-variables/
string(COMPARE EQUAL "$ENV{APPVEYOR}" "True" is_appveyor)

string(COMPARE EQUAL "$ENV{GITHUB_USER_PASSWORD}" "" password_is_empty)

if((is_travis OR is_appveyor) AND NOT password_is_empty)
  option(HUNTER_RUN_UPLOAD "Upload cache binaries" ON)
endif()

set(
    HUNTER_PASSWORDS_PATH
    "${CMAKE_CURRENT_LIST_DIR}/cmake/Hunter/passwords.cmake"
    CACHE
    FILEPATH
    "Hunter passwords"
)

include(cmake/HunterGate.cmake)
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/145caad39cc45be91ec431ddcb6df53d6a7206e4.zip"
    SHA1 "c9fb942fccc579586f810ac413f4816cc792d2da"
)

project(foo)

hunter_add_package(sugar)
