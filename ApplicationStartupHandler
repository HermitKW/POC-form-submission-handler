package hk.sfc.eds.formsubmissionservice;

import java.time.LocalDateTime;
import java.util.HashMap;
import java.util.Map;
import java.util.UUID;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.context.event.ApplicationPreparedEvent;
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.ApplicationContextException;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;
import hk.sfc.eds.formsubmissionservice.api.GeneralSubmissionController;

@Component
public class ApplicationStartupHandler {

  private static final Logger logger = LoggerFactory.getLogger(ApplicationStartupHandler.class);

  @Autowired
  protected RabbitTemplate rabbitTemplate;
  
  @EventListener(ApplicationPreparedEvent.class)
  public void onApplicationPrepared(ApplicationPreparedEvent event) {
    logger.info("Event Handler - Start: ApplicationPreparedEvent");
    
    String healthCheckId = LocalDateTime.now().toString() + ":" + UUID.randomUUID().toString();
    logger.info("posting health check message to mq, healthCheckId: {}", healthCheckId);
    
    Map<String, String> message = new HashMap<>();
    message.put("healthCheckId", healthCheckId);

    rabbitTemplate.convertAndSend("eds-form-submission", message);
    
    logger.info("posted health check message, healthCheckId: {}", healthCheckId);
    
    logger.info("Event Handler - Finish: ApplicationPreparedEvent");
  }
  
  @EventListener(ApplicationReadyEvent.class)
  public void onApplicationReady(ApplicationReadyEvent event) {
    logger.info("Event Handler - Start: ApplicationReadyEvent");

    try {
      GeneralSubmissionController generalSubmissionController = (GeneralSubmissionController) event
          .getApplicationContext().getBean("generalSubmissionController");

//      generalSubmissionController.preloadFormConfig();
      generalSubmissionController.preloadModuleSequence();
    } 
    catch (Exception ex) {
      throw new ApplicationContextException("Failed to preload form config", ex);
    }
    
    logger.info("Event Handler - Finish: ApplicationReadyEvent");
  }

}


 protected void sendMessage(String submissionId) {
    logger.debug("------------ sending: {}", submissionId);

    Map<String, String> message = new HashMap<>();
    message.put("submissionId", submissionId);

    rabbitTemplate.convertAndSend("eds-form-submission", message);
  }



 if (commitOnUpload) {
        // call workflow dispatch
        dispatchWorkflow(submissionId, "SUBMIT");
        
        // Add message queue record
        sendMessage(submissionId);
      }



if (currentStep == null || "END".equalsIgnoreCase(currentStep) || currentStep.equalsIgnoreCase(previousStep)) {
            break;
          }

          switch (currentStep) {
            case FORM_STEP_COPY_TO_INT_DB:
              isSuccess = moveToIntDB(submissionId);
              break;
            case FORM_STEP_DM:
              isSuccess = dmController.initStoreDMS(submissionId);
              break;
            case FORM_STEP_EMAIL_INT:
              isSuccess = emailController.initSendIntEmail(submissionId);
              break;
            case FORM_STEP_EMAIL_EXT:
              isSuccess = emailController.initSendExtEmail(submissionId);
              break;
            case FORM_STEP_DEL_EXT_DB:
              isSuccess = removeFileAtExtDB(submissionId);
              break;
            case FORM_STEP_DEL_INT_DB:
              isSuccess = removeFileAtIntDB(submissionId);
              break;
            case FORM_STEP_SEND_PORTAL_MSG:
              isSuccess = messageController.initSendPortalMessage(submissionId);
              break;
            case FORM_STEP_TRANSFORM:
              isSuccess = transformSubmissionFile(submissionId);
              break;
            case FORM_STEP_M3:
              isSuccess = m3Controller.m3integration(submissionId);
              break;
            case FORM_STEP_FORWARD_DATA:
              isSuccess = forwardDataController.forwardData(submissionId);
              break;
            case FORM_STEP_POPULATE_SUBMIT:
              isSuccess = populateSubmissionStatus(submissionId);
              break;
            case FORM_STEP_POPULATE_FORM_DATA:
              isSuccess = populateFormData(submissionId);
              break;
            case FORM_STEP_VALIDATE_TOPICS:
              isSuccess = validateTopicsController.validate(submissionId);
              break;
            case FORM_STEP_DM_DFC:
              isSuccess = dmDfcController.importFile(submissionId);
              break;
            case FORM_STEP_DM_DFC_FRR:
              isSuccess = dmDfcController.importFileFRR(submissionId);
              break;
            case FORM_STEP_EXPORT_DOC_PROP:
              isSuccess = dmDfcController.exportDocProp(submissionId);
              break;
            case FORM_STEP_REMOVE_REQ_ATM:
              isSuccess = removeRequestAttachment(submissionId);
              break;
            case FORM_STEP_CUSTOM_PROCESS:
              isSuccess = customProcess(submissionId);
              break;
            case FORM_STEP_CUSTOM_PROCESS_EXT:
              isSuccess = customProcess(submissionId, true);
              break;
            case FORM_STEP_CUSTOM_PROCESS_INT:
              isSuccess = customProcess(submissionId, false);
              break;
            case FORM_STEP_PEGA_CALLBACK:
              isSuccess = pegaCallabackController.pegaCallback(submissionId);
              break;
            case FORM_STEP_UPDATE_CUSTOM_CONFIG:
              isSuccess = updateCustomConfig(submissionId);
              break;
            case FORN_STEP_FRR_RISK_ALERT:
              isSuccess = frrController.riskAlertProcess(submissionId);
              break;
            case FORN_STEP_FRR_DATA_INSERT:
              isSuccess = frrController.dataInsertProcess(submissionId);
              break;
            case FORN_STEP_FRR_UPLOAD_DATA:
                isSuccess = frrUploadDataByAdmin(submissionId);
                break;
            case FORN_STEP_STAR_DATA_INSERT:
                isSuccess = frrController.dataInsertProcessStar(submissionId);
                break;
            case FORM_STEP_ALPS_PROCESS:
              isSuccess = alpsProcess(submissionId);
              break;
            case FORM_STEP_CHECK_EOD:
              isSuccess = checkEOD(submissionId);
              break;
            case FORM_STEP_CHECK_PAUSE_PERIOD:
              isSuccess = checkPausePeriod(submissionId);
              break;
            case FORM_STEP_CHECK_PAUSE_COND_EXT:
              isSuccess = checkPauseCondition(submissionId, true);
              break;
            case FORM_STEP_CHECK_PAUSE_COND_INT:
              isSuccess = checkPauseCondition(submissionId, false);
              break;
            case FORM_STEP_CUSTOM_STATUS:
              isSuccess = frrController.customStatus(submissionId);
              break;
            case FORM_STEP_SEND_PROTECTED_NOTI:
              isSuccess = protectedNotiController.sendProtectedNoti(submissionId);
              break;
            default:
              break;
          }

          previousStep = currentStep;
          logger.debug("previousStep: {}", previousStep);
        }
      } catch (Exception ex) {
        logger.error("FormSubmissionListener.receive exception: ", ex);
      }
