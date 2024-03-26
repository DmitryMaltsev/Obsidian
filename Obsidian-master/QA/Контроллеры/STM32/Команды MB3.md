1. В файле src/app/api-impl.c есть ф-ия bool mb_write_cmd(uint16_t v) 
2. stepdrv.cmd = КОД_КОМАНДЫ, где КОД_КОМАНДЫ - число от 0 до 65535,
3. src/app/task_mb3.c строка 51: if (ln <= 0) {
4. CMD_TURN_ON:
5. #include "main.h"