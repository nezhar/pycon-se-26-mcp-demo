RUN apt-get update && apt-get install -y --no-install-recommends socat \
    && rm -rf /var/lib/apt/lists/*
RUN printf '%s\n' '#!/bin/sh' \
    'socat TCP-LISTEN:3000,bind=127.0.0.1,fork,reuseaddr TCP:vibepod-board:3000 &' \
    'socat TCP-LISTEN:8766,fork,reuseaddr TCP:127.0.0.1:8765 &' \
    'exec /usr/local/bin/entrypoint.sh "$@"' > /usr/local/bin/board-forward.sh \
    && chmod +x /usr/local/bin/board-forward.sh
ENTRYPOINT ["/usr/local/bin/board-forward.sh"]
CMD ["pi"]

