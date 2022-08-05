FROM ruby:3.1.2 as builder

ENV TZ=Asia/Tokyo
ENV RAILS_ENV=production
ENV RAILS_LOG_TO_STDOUT=1

# ARG BUILD_PACKAGES="alpine-sdk build-base linux-headers mariadb-dev"
ARG BUNDLE_OPTIONS="--jobs 4 --deployment --without development test"

WORKDIR /app

# COPY Gemfile* /
# COPY vendor/cache /vendor/cache
COPY . /app

RUN bundle config set path /vendor/bundle && \
    bundle install ${BUNDLE_OPTIONS}

RUN --mount=type=secret,id=RAILS_MASTER_KEY \
  bundle exec rails assets:precompile RAILS_MASTER_KEY=$(cat /run/secrets/RAILS_MASTER_KEY)

CMD ["bundle", "exec" , "puma"]

# FROM ruby:${RUBY_VERSION}-alpine${ALPINE_VERSION}

# ARG BUNDLER_VERSION
# ARG BASE_PACKAGES
# ARG RUN_PACKAGES="execline mariadb-client-libs openrc"
# ARG APP_ROOT=/app
# ARG APP_PORT=3000
# ARG TIME_ZONE=Asia/Tokyo
# # Rubyの公式イメージの設定を上書きする
# ENV BUNDLE_APP_CONFIG ${APP_ROOT}/.bundle

# WORKDIR ${APP_ROOT}

# # 成果物だけをビルドステージから持ってくる
# COPY --from=builder /vendor/bundle ${APP_ROOT}/vendor/bundle
# COPY --from=builder /usr/local/bundle ${BUNDLE_APP_CONFIG}
# COPY . ${APP_ROOT}

# RUN apk add --no-cache ${BASE_PACKAGES} ${RUN_PACKAGES} ＆＆ \
#     cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime ＆＆ \
#     gem install bundler --version ${BUNDLER_VERSION} --no-document ＆＆ \
#     bundle config set path ${APP_ROOT}/vendor/bundle

# # UnicornのSocketファイルをtmp配下に置くためNginxのコンテナからアクセスできるようにする
# VOLUME ${APP_ROOT}/tmp
# VOLUME ${APP_ROOT}/public

# EXPOSE ${APP_PORT}

# # プロセスの起動はシェルスクリプトにまとめている
# ENTRYPOINT ["./entrypoint.sh"]
