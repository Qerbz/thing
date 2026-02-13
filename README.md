#!/bin/bash
SCREEN_W=$(xdpyinfo | grep dimensions | awk '{print $2}' | cut -d'x' -f1)
SCREEN_H=$(xdpyinfo | grep dimensions | awk '{print $2}' | cut -d'x' -f2)
HALF_W=$((SCREEN_W / 2))
HALF_H=$((SCREEN_H / 2))

URLS=(
    "https://olem.dashboard.enmo.ai"
    "https://lang.dashboard.enmo.ai"
    "https://hp.dashboard.enmo.ai"
    "https://fjelly.dashboard.enmo.ai"
)
POSITIONS=("0,0" "0,$HALF_H" "$HALF_W,0" "$HALF_W,$HALF_H")

for i in 0 1 2 3; do
    chromium \
        --app="${URLS[$i]}" \
        --user-data-dir="/tmp/chrome$i" \
        --window-position="${POSITIONS[$i]}" \
        --window-size="$HALF_W,$HALF_H" \
        --noerrdialogs \
        --disable-infobars \
        --no-first-run \
        --disable-features=TranslateUI &
    sleep 1
done

sleep 4
for WID in $(xdotool search --class "Chromium"); do
    xprop -id $WID -f _MOTIF_WM_HINTS 32c -set _MOTIF_WM_HINTS "0x2, 0x0, 0x0, 0x0, 0x0"
done
