cmake_minimum_required(VERSION 3.10)

# Define the project directory for installing training data files
set(DATA_INSTALL_DIR "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/training")

# List all training text files
set(TRAINING_FILES
    training_albanian_SQ.txt
    training_basque_ES.txt
    training_bengali_BD.txt
    training_canna_JP.txt
    training_czechC_CS.txt
    training_czech_CS.txt
    training_danish_DK.txt
    training_dutch_NL.txt
    training_englishLC_GB.txt
    training_english_GB.txt
    training_finnish_FI.txt
    training_frenchC_FR.txt
    training_french_FR.txt
    training_german_DE.txt
    training_greek_GR.txt
    training_hebrew_IL.txt
    training_hiragana60_JP.txt
    training_hiragana83_JP.txt
    training_hungarian_HU.txt
    training_italianC_IT.txt
    training_italian_IT.txt
    training_mongolian_MN.txt
    training_persian_IR.txt
    training_polish_PL.txt
    training_portugueseC_BR.txt
    training_portuguese_BR.txt
    training_russian_RU.txt
    training_spanishC_ES.txt
    training_spanish_ES.txt
    training_spyNew.txt
    training_swahili_KE.txt
    training_swedish_SE.txt
    training_turkish_TR.txt
    training_welsh_GB.txt
)

# Install training files to the appropriate data directory
install(FILES ${TRAINING_FILES}
    DESTINATION ${DATA_INSTALL_DIR}
)

# List extra distribution files (these files are included for distribution but typically not installed)
set(EXTRA_DIST
    training_czech_CS.COPYING
    training_welsh_GB.COPYING
)

# Print message or log the extra distribution files; actual installation or packaging
# actions depend on specific project requirements or packaging scripts.
foreach(EXTRA_FILE ${EXTRA_DIST})
    message(STATUS "Extra dist file: ${EXTRA_FILE} included in the project for reference.")
endforeach()

# Optionally, you could install these files if needed somewhere with the project.
# Uncomment and modify the following line according to actual needs:
# install(FILES ${EXTRA_DIST} DESTINATION "${CMAKE_INSTALL_DATADIR}/${PROJECT_NAME}/training/legal")
