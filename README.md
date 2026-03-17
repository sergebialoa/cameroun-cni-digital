import app from './app';
import { Logger } from './utils/logger';

const PORT = process.env.PORT || 3000;
const NODE_ENV = process.env.NODE_ENV || 'development';

const server = app.listen(PORT, () => {
  Logger.info(`✅ Server running on port ${PORT} in ${NODE_ENV} mode`);
  Logger.info(`📍 API URL: http://localhost:${PORT}`);
  Logger.info(`🗄️  Database: ${process.env.DATABASE_URL?.split('@')[1]}`);
});

// Graceful shutdown
process.on('SIGTERM', () => {
  Logger.info('SIGTERM signal received: closing HTTP server');
  server.close(() => {
    Logger.info('HTTP server closed');
    process.exit(0);
  });
});

process.on('SIGINT', () => {
  Logger.info('SIGINT signal received: closing HTTP server');
  server.close(() => {
    Logger.info('HTTP server closed');
    process.exit(0);
  });
});

export default server;
