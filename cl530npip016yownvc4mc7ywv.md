## Introduction to Logging in Python

This code snippet helps setup base logging for your applications in python.
- Logging Format helps set the format of your logs
- File name helps save the logs in a txt file. File format can be empty if you just want logs in the CLI
- Level helps limit the logged information to the level of information you are interested in logging


There are various logging levels to be aware of
- DEBUG: Detailed information, typically of interest only when diagnosing problems

- INFO: Confirmation that things are working as expected

- WARNING: An indication that something unexpected happened, or indicative of some problem in the near future (e.g. ‘disk space low’). The software is still working as expected

- ERROR: Due to a more serious problem, the software has not been able to perform some function

- CRITICAL: A serious error, indicating that the program itself may be unable to continue running


```py
import logging

FILENAME = "filepath for your logging file.txt"
LOG_FORMAT = "%(levelname)s %(asctime)s %(message)s"
logging.basicConfig(filename=FILENAME,
                    level=logging.DEBUG,
                    format=LOG_FORMAT)

logger = logging.getLogger()


def logging_test():    
    logger.debug("Debug")
    logger.info("Info")
    logger.warning("Warning")
    logger.error("Error")
    logger.critical("Critical")
    return None
``` 
