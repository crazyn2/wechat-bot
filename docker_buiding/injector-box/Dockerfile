FROM sureer/wine-box:8.0.2
COPY target/root/ /
# init target with GUI
RUN bash -c 'nohup /entrypoint.sh 2>&1 &' && sleep 5 && /payloads.sh \
    && sudo rm /tmp/.X0-lock
# clear payloads
RUN sudo rm -r /payloads
COPY root/ /
# init injector with GUI
RUN bash -c 'nohup /entrypoint.sh 2>&1 &' && sleep 5 && /payloads.sh \
    && sudo rm /tmp/.X0-lock \
    && (sudo cp -r /*-etc/* /etc || true) \
    && (sudo chown -R app:app /drive_c && cp -r /drive_c/* /home/app/.wine/drive_c/ || true)

#settings
ENTRYPOINT ["/inj-entrypoint.sh"]
